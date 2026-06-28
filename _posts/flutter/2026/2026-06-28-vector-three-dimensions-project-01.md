---
title: Mathematics - Euclidean distance(유클리드 거리) 및 3D 투영 매트릭스 구현
date: 2026-06-28 16:00:00 +0900
category: [flutter]
tag: [flutter, 3d, math]
---

- [**목표**](#목표)
  - [**이유**](#이유)
- [**구성**](#구성)
  - [**1. 큐브 만들기**](#1-큐브-만들기)
  - [**2. 3D를 2D View로 전환 (투영 파이프라인)**](#2-3d를-2d-view로-전환-투영-파이프라인)
  - [**3. 카메라 (구면좌표계 시점 제어)**](#3-카메라-구면좌표계-시점-제어)
  - [**4. View 제작 (인터랙션 및 렌더링 레이어)**](#4-view-제작-인터랙션-및-렌더링-레이어)

---

# **목표**

3D 대상을 Flutter 기반 환경에서 외부 3D 에셋이나 GPU 가속 렌더러 없이 순수 수학적 행렬 연산과 코드로만 구현.

## **이유**

일반적인 3D 모델 엔진은 수많은 폴리곤 데이터를 기반으로 구동되므로 파일 용량이 커지고, 런타임에서 GPU 오버헤드를 유발한다. 디바이스 사양에 의존하지 않는 정교한 차원 변환 수학(Projection Matrix)과 상태 관리를 구축하면, 가벼운 벡터 데이터 플로팅만으로 화면에 완벽한 3D 와이어프레임을 연산하고 제어할 수 있다.

---

# **구성**

## **1. 큐브 만들기**

3D 공간을 정의하는 기본 단위는 정점(Vertex) 데이터다. 원점을 중심으로 균등하게 뻗어나가는 정육면체의 꼭짓점 8개를 수학적으로 생성한다.

```dart
// sample_geometry.dart
import 'dart:math';
import 'package:vector_math/vector_math_64.dart';

class SampleGeometry {
  /// 지정된 크기(size)를 가진 정육면체의 8개 절대 좌표 정점을 생성한다.
  List<Vector3> createCube(double size) {
    final double half = size / 2.0;
    return [
      // 앞면 (Front Face) - Z축 음수 영역
      Vector3(-half, -half, -half), // 0번: 좌측 하단 앞
      Vector3(half, -half, -half),  // 1번: 우측 하단 앞
      Vector3(half, half, -half),   // 2번: 우측 상단 앞
      Vector3(-half, half, -half),  // 3번: 좌측 상단 앞

      // 뒷면 (Back Face) - Z축 양수 영역
      Vector3(-half, -half, half),  // 4번: 좌측 하단 뒤
      Vector3(half, -half, half),   // 5번: 우측 하단 뒤
      Vector3(half, half, half),    // 6번: 우측 상단 뒤
      Vector3(-half, half, half),   // 7번: 좌측 상단 뒤
    ];
  }
}
```

### **해설**
3차원 공간 내부의 정점은 $(x, y, z)$ 독립 변수의 조합으로 결정된다. 형태의 왜곡이 없는 정육면체를 빌드하기 위해 3D 절대 공간의 중심 원점 $(0.0, 0.0, 0.0)$을 정육면체의 무게중심으로 설정한다. 한 변의 길이가 `size`라면 중심으로부터 각 축의 양수와 음수 방향으로 정확히 절반인 `half`($\frac{\text{size}}{2.0}$)만큼 이동해야 정합성이 유지된다.

정육면체의 꼭짓점 개수는 각 축 변환 상태의 가짓수인 $2(\text{X축}) \times 2(\text{Y축}) \times 2(\text{Z축}) = 8$개가 되며, 이 정형화된 데이터 배열의 인덱스를 선으로 연결하여 와이어프레임을 렌더링한다.

---

## **2. 3D를 2D View로 전환 (투영 파이프라인)**

컴퓨터 메모리에 존재하는 3차원 정점 데이터를 2차원 평면 디스플레이 스크린 좌표로 변환하기 위해 4x4 행렬 곱셈 연산 기하학을 적용한다. 이 과정을 투영 파이프라인(Projection Pipeline)이라고 한다.

```dart
// evaluator.dart
import 'dart:ui';
import 'package:vector_math/vector_math_64.dart';

/// 3D 공간 좌표를 2D 스크린 픽셀 오프셋으로 변환하는 매트릭스 연산 클래스
class TdpEvaluator {
  final Matrix4 _viewMatrix = Matrix4.identity();
  final Matrix4 _projectionMatrix = Matrix4.identity();
  final Matrix4 _viewportMatrix = Matrix4.identity();

  /// 카메라 상태 및 화면 크기 변화에 맞춰 변환 행렬들을 동적으로 갱신한다.
  void updateMatrices(Vector3 cameraPosition, Vector3 cameraTarget, Size screenSize) {
    // 1. View Matrix 계산: 카메라 위치(Eye), 주시점(Target), 상방 벡터(Up-Vector) 기반
    setViewMatrix(_viewMatrix, cameraPosition, cameraTarget, Vector3(0.0, 1.0, 0.0));

    // 2. Projection Matrix 계산: 시야각 60도, 화면 종횡비, Near/Far 클리핑 평면 지정
    setPerspectiveMatrix(
      _projectionMatrix,
      radians(60.0),
      screenSize.width / screenSize.height,
      0.1,
      1000.0,
    );

    final double halfW = screenSize.width / 2.0;
    final double halfH = screenSize.height / 2.0;

    // 3. Viewport Matrix 계산: [-1, 1] 클립 공간을 스크린 픽셀 좌표계로 매핑 및 Y축 반전
    _viewportMatrix.setIdentity();
    _viewportMatrix.setRow(0, Vector4(halfW, 0.0, 0.0, halfW));
    _viewportMatrix.setRow(1, Vector4(0.0, -halfH, 0.0, halfH));
    _viewportMatrix.setRow(2, Vector4(0.0, 0.0, 1.0, 0.0));
    _viewportMatrix.setRow(3, Vector4(0.0, 0.0, 0.0, 1.0));
  }

  /// 하나의 3D 점을 수학적 투영을 거쳐 2D 스크린 좌표(Offset)로 출력한다.
  Offset projectPoint(Vector3 point3D) {
    // 동차 좌표계(Homogeneous Coordinates) 변환을 위해 w 성분에 1.0 주입
    final Vector4 homogeneousPoint = Vector4(point3D.x, point3D.y, point3D.z, 1.0);

    // 월드 좌표 -> 뷰 공간 -> 클립 공간 순으로 행렬 곱셈 연산 수행
    final Vector4 transformed = _projectionMatrix * _viewMatrix * homogeneousPoint;

    // Zero Division 방지를 위한 예외 처리
    if (transformed.w == 0.0) return Offset.zero;

    // 원근 분할(Perspective Divide): 동차 좌표를 정규화된 디바이스 좌표(NDC)로 변환
    final Vector3 ndc = Vector3(
      transformed.x / transformed.w,
      transformed.y / transformed.w,
      transformed.z / transformed.w,
    );

    // 뷰포트 행렬 연산을 통해 최종 픽셀 좌표계 도출
    final Vector4 screenPos = _viewportMatrix * Vector4(ndc.x, ndc.y, ndc.z, 1.0);
    return Offset(screenPos.x, screenPos.y);
  }
}
```

### **해설**
3차원 좌표가 화면에 투영되는 파이프라인 연산은 세 단계의 선형대수학 매트릭스를 관통한다.

1. **뷰 행렬 (View Matrix)**: 가상의 세계(World Space) 기준 좌표계를 카메라 중심의 기준 좌표계(Camera Space)로 치환한다. 카메라의 절대 위치, 바라보는 타깃 벡터, 세계의 수직 축 기준점 지정을 유기적으로 결합하여 생성한다.
2. **투영 행렬 (Projection Matrix)**: 시야각(FOV)과 초점 거리를 기반으로 원근감을 부여하는 원근 투영(Perspective Projection)을 수행한다. 이 행렬을 통과하면 원근 법칙에 따라 먼 객체는 중심부로 수렴하고 가까운 객체는 바깥으로 발산하는 동차 공간(Clip Space)이 형성되며, 원근 데이터는 변환된 4차원 벡터의 $W$ 컴포넌트에 보존된다.
3. **원근 분할 및 뷰포트 변환 (Perspective Divide & Viewport Matrix)**: 4차원 벡터 데이터를 스크린의 평면으로 안착시키는 공식이다. 계산된 벡터의 $(X, Y, Z)$ 요소를 $W$ 값으로 나누는 원근 분할 과정을 거치면 $[-1.0, 1.0]$ 범위를 가지는 NDC(Normalized Device Coordinates) 공간으로 정규화된다. 마지막으로 뷰포트 행렬을 곱해 픽셀 단위 크기로 확장한다. 이때 그래픽스 좌표계의 상하 반전 특성을 상쇄하기 위해 $Y$ 성분에 음수를 부여하여 중심점 보정 작업을 완료한다.

---

## **3. 카메라 (구면좌표계 시점 제어)**

오브젝트 주위를 공전(Orbiting)하며 관찰하는 입체적인 카메라 컨트롤을 구현하기 위해 구면좌표계(Spherical Coordinate System) 수학을 기반으로 한 상태 관리 아키텍처를 설계한다.

```dart
// camera_3d_model.dart
import 'package:freezed_annotation/freezed_annotation.dart';
import 'package:vector_math/vector_math_64.dart';

part 'camera_3d_model.freezed.dart';

@freezed
abstract class Camera3DModel with _$Camera3DModel {
  const factory Camera3DModel({
    required Vector3 position, // 직교좌표계 상의 카메라 X, Y, Z 위치 벡터
    required Vector3 target,   // 카메라가 고정하여 바라보는 타깃 지점
    required double radius,    // 타깃으로부터 카메라까지의 직선 거리 (줌 레벨)
    required double theta,     // 수평 회전각 (Azimuth Angle)
    required double phi,       // 수직 회전각 (Zenith/Elevation Angle)
  }) = _Camera3DModel;
}
```

```dart
// camera_3d_notifier.dart
import 'dart:math';
import 'package:riverpod_annotation/riverpod_annotation.dart';
import 'package:vector_math/vector_math_64.dart';
import '../../models/camera/camera_3d_model.dart';

part 'camera_3d_notifier.g.dart';

@riverpod
class Camera3DNotifier extends _$Camera3DNotifier {
  @override
  Camera3DModel build() {
    return Camera3DModel(
      position: Vector3(0.0, 5.0, 10.0),
      target: Vector3.zero(),
      radius: 12.0,
      theta: 0.0,
      phi: pi / 4, // 45도 위에서 아래로 내려다보는 시선
    );
  }

  /// 마우스 및 터치 드래그 변위값에 따라 카메라의 수평/수직 각도를 갱신하고 위치를 재계산한다.
  void rotateCamera(double deltaX, double deltaY) {
    final double newTheta = state.theta - (deltaX * 0.005);
    // 화면 뒤집힘 현상(Gimbal Lock) 및 오버플로를 방지하기 위해 상하 각도를 0.1 ~ pi/2 임계 영역 내로 제한한다.
    final double newPhi = (state.phi + (deltaY * 0.005)).clamp(0.1, pi / 2 - 0.1);

    // 삼각함수를 통한 구면좌표계 -> 직교좌표계(Cartesian Coordinates) 역변환 공식 적용
    final double x = state.radius * sin(newPhi) * sin(newTheta);
    final double y = state.radius * cos(newPhi);
    final double z = state.radius * sin(newPhi) * cos(newTheta);

    state = state.copyWith(
      theta: newTheta,
      phi: newPhi,
      position: Vector3(x, y, z),
    );
  }

  /// 마우스 휠 스크롤 입력을 받아 카메라의 공전 반경 거리(Radius)를 가감한다.
  void zoomCamera(double deltaScroll) {
    final double newRadius = (state.radius + (deltaScroll * 0.01)).clamp(3.0, 30.0);

    final double x = newRadius * sin(state.phi) * sin(state.theta);
    final double y = newRadius * cos(state.phi);
    final double z = newRadius * sin(state.phi) * cos(state.theta);

    state = state.copyWith(
      radius: newRadius,
      position: Vector3(x, y, z),
    );
  }
}
```

### **해설**
카메라 주위를 직접 직교좌표계 $(X, Y, Z)$ 단위로 조작하여 공전 궤도를 그리는 행위는 정밀한 삼각비 제어 없이는 불가능하다. 따라서 카메라 연산은 반지름 $r$(Distance), 수평 회전각 $\theta$(Theta), 수직 앙각 $\phi$(Phi)라는 독립 변수를 가지는 구면좌표계 시스템 내부에서 수치 변화를 처리하는 구조가 이상적이다.

사용자의 드래그 제스처 변화량이 입력되면 각도 제어 변수인 `theta`와 `phi`가 가감되며, 화면이 완전히 뒤집어져 뷰 행렬의 수직 축이 붕괴하는 현상을 막기 위해 `clamp` 처리를 수행하여 안전 반경을 유지한다. 데이터 갱신의 종착점에서는 변경된 구면좌표계 인자들을 바탕으로 아래의 삼각함수 물리 공식을 실행해 직교좌표계 $\text{Vector3}(x, y, z)$ 포지션 벡터로 완벽히 역산해 낸다.

$$x = r \times \sin(\phi) \times \sin(\theta)$$
$$y = r \times \cos(\phi)$$
$$z = r \times \sin(\phi) \times \cos(\theta)$$

---

## **4. View 제작 (인터랙션 및 렌더링 레이어)**

수학적으로 완성된 파이프라인과 카메라 위치 상태(State)를 결합하여 화면에 드로잉 처리를 하고 사용자 이벤트를 캡처하는 UI 아키텍처를 구현한다.

```dart
// wire_frame_painter.dart
import 'package:flutter/material.dart';
import 'package:vector_math/vector_math_64.dart' as v_math;
import '../../core/evaluator/evaluator.dart';
import '../../models/sample/geometry/sample_geometry.dart';

class WireframePainter extends CustomPainter {
  final v_math.Vector3 cameraPosition;
  final v_math.Vector3 cameraTarget;
  final TdpEvaluator _evaluator = TdpEvaluator();
  final SampleGeometry _geometry = SampleGeometry();

  WireframePainter({
    required this.cameraPosition,
    required this.cameraTarget,
  });

  @override
  void paint(Canvas canvas, Size size) {
    // 1. 현재 뷰포트 크기와 카메라 벡터로 변환 매트릭스 동적 갱신
    _evaluator.updateMatrices(cameraPosition, cameraTarget, size);

    // 2. 3D 공간상의 큐브 기본 정점 8개 빌드
    final List<v_math.Vector3> cubeVertices = _geometry.createCube(4.0);

    // 3. 투영 파이프라인 매핑 연산을 루프 처리하여 2D 픽셀 오프셋 리스트로 전량 치환
    final List<Offset> screenPoints = cubeVertices.map((v) => _evaluator.projectPoint(v)).toList();

    final Paint paint = Paint()
      ..color = Colors.greenAccent
      ..strokeWidth = 2.0
      ..style = PaintingStyle.stroke;

    // 4. 추출된 스크린 오프셋 정점 인덱스를 상호 연결하여 와이어프레임 렌더링 실행
    if (screenPoints.length >= 8) {
      // 앞면 사각형 드로잉 (정점 0, 1, 2, 3 연결)
      canvas.drawLine(screenPoints[0], screenPoints[1], paint);
      canvas.drawLine(screenPoints[1], screenPoints[2], paint);
      canvas.drawLine(screenPoints[2], screenPoints[3], paint);
      canvas.drawLine(screenPoints[3], screenPoints[0], paint);

      // 뒷면 사각형 드로잉 (정점 4, 5, 6, 7 연결)
      canvas.drawLine(screenPoints[4], screenPoints[5], paint);
      canvas.drawLine(screenPoints[5], screenPoints[6], paint);
      canvas.drawLine(screenPoints[6], screenPoints[7], paint);
      canvas.drawLine(screenPoints[7], screenPoints[4], paint);

      // 앞면과 뒷면의 대응하는 꼭짓점을 연결하여 기둥 형성
      canvas.drawLine(screenPoints[0], screenPoints[4], paint);
      canvas.drawLine(screenPoints[1], screenPoints[5], paint);
      canvas.drawLine(screenPoints[2], screenPoints[6], paint);
      canvas.drawLine(screenPoints[3], screenPoints[7], paint);
    }
  }

  @override
  bool shouldRepaint(covariant WireframePainter oldDelegate) {
    // 카메라 좌표 변경이 감지되면 Canvas 무효화 후 재지정 드로잉 처리 지시
    return oldDelegate.cameraPosition != cameraPosition || oldDelegate.cameraTarget != cameraTarget;
  }
}
```

```dart
// screen.dart
import 'package:flutter/material.dart';
import 'package:flutter/gestures.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import '../painter/wire_frame/wire_frame_painter.dart';
import '../providers/notifier/camera_3d_notifier.dart';

class ThreeDimensionalScreenConsumer extends ConsumerWidget {
  const ThreeDimensionalScreenConsumer({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // 카메라 상태 구독: 변화 발생 시 자동으로 리빌드 수행
    final cameraState = ref.watch(camera3DProvider);

    return Listener(
      onPointerSignal: (pointerSignal) {
        if (pointerSignal is PointerScrollEvent) {
          // 마우스 휠 변위 분석을 통한 실시간 줌인/줌아웃 바인딩
          ref.read(camera3DProvider.notifier).zoomCamera(pointerSignal.scrollDelta.dy);
        }
      },
      child: GestureDetector(
        onPanUpdate: (details) {
          // 화면 드래그 변위값을 수집하여 카메라 상태 노티파이어 공전 제어 함수로 이관
          ref.read(camera3DProvider.notifier).rotateCamera(
            details.delta.dx,
            details.delta.dy,
          );
        },
        child: Container(
          color: Colors.black, // 히트 테스트 영역 확보 및 우주 공간 배경 선언
          width: double.infinity,
          height: double.infinity,
          child: CustomPaint(
            painter: WireframePainter(
              cameraPosition: cameraState.position,
              cameraTarget: cameraState.target,
            ),
          ),
        ),
      ),
    );
  }
}
```

---