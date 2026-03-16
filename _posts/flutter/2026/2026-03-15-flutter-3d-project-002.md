<!-- ---
title: Flutter - 3D project 01 - camera 02
date: 2026-03-14 14:53:00 +/-09:00
categories: [Flutter, 3D]
tags: [flutter, 3d, camera]
---
- [**카메라**](#카메라)
  - [**카메라 모델의 구성 요소**](#카메라-모델의-구성-요소)
- [**코드**](#코드)

# **카메라 모델의 구성 요소**

카메라를 코드로 구현하기 위해서는 단순히 위치만 아는 것으로는 부족하다. 3D 엔진이 화면을 그리기 위해 필요한 최소한의 데이터는 다음과 같다.

### **카메라 모델 구성 요소**

| 분류 | 항목 | 심볼 | 타입 | 설명 |
| :--- | :--- | :---: | :---: | :--- |
| **상태 변수** | **Position** | $C_{pos}$ | `double` | 카메라의 현재 위치 $(x, y, z)$ |
| | **Target** | $T_{pos}$ | `double` | 카메라가 바라보는 지점의 좌표 |
| | **Up Vector** | $V_{up}$ | `Vector3` | 카메라의 상단 방향 (화면 뒤집힘 방지) |
| | **FOV** | $FOV$ | `double` | 시야각 (field of view) |
| **변환 행렬** | **View Matrix** | $M_{view}$ | `Matrix4` | 월드 좌표를 카메라 시점으로 변환하는 행렬 |
| | **Projection Matrix** | $M_{proj}$ | `Matrix4` | 3D 좌표에 원근감을 부여하여 2D로 투영하는 행렬 |

### **코드**
freezed의 annotation을 사용해서 자동화할 것이다.
명령어는 `dart run build_runner build`다.

```dart
// path: model/camera/camera_model.dart

@freezed
abstract class CameraModel with _$CameraModel {
  const factory CameraModel({
    /// 카메라의 현재 위치 좌표 (x, y, z)
    required Vector3 position,

    /// 카메라가 정면으로 바라보고 있는 주시점 좌표
    required Vector3 target,

    /// 카메라의 상단 방향을 정의하는 벡터 (화면의 회전 기준)
    required Vector3 up,

    /// 시야각 (Field of View, 도 단위)
    required double fov,

    /// 렌더링이 시작되는 최소 거리 (Near Clipping Plane)
    required double near,

    /// 렌더링이 제한되는 최대 거리 (Far Clipping Plane)
    required double far,

    /// 화면의 가로세로 비율 (Width / Height)
    required double aspectRatio,
  }) = _CameraModel;

  const CameraModel._();

  /// 월드 좌표계를 카메라 중심 좌표계로 변환하는 뷰 행렬
  Matrix4 get viewMatrix {
    return makeViewMatrix(position, target, up);
  }

  /// 3D 좌표에 원근감을 적용하여 2D 화면으로 투영하는 행렬
  Matrix4 get projectionMatrix {
    return makePerspectiveMatrix(
      radians(fov),
      aspectRatio,
      near,
      far,
    );
  }
}
```
 -->
