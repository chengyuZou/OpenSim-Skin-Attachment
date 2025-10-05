# OpenSim-Skin-Attachment

now(unfinished)
![OpenSim 4 5 2025_3_23 22_32_55](https://github.com/user-attachments/assets/eeefac02-0446-45c3-962a-0388f0c68571)

```python
@Article{Pagnon_2022_JOSS, 
  AUTHOR = {Pagnon, David and Domalain, Mathieu and Reveret, Lionel}, 
  TITLE = {Pose2Sim: An open-source Python package for multiview markerless kinematics}, 
  JOURNAL = {Journal of Open Source Software}, 
  YEAR = {2022},
  DOI = {10.21105/joss.04362}, 
  URL = {https://joss.theoj.org/papers/10.21105/joss.04362}
}
```
给出的main文件为.stl格式的皮肤模型

重要提示: 凡是在<Body>标签中有"skin"名字的都要改它的<mesh_file>，不然报错或者皮肤模型不显示

## 1. 简介
OpenSim 是斯坦福大学开发的用于开发、分析和可视化人体肌肉骨骼系统的免费开源软件。它能应用在很多领域，如行走动力学分析、运动表现研究、手术过程仿真、医疗器械设计等。在OpenSim中，一个肌肉骨骼模型是由各个关节把多块骨骼连接起来，其中肌肉附着在骨骼上，通过肌肉产生的力来带动关节运动。目前OpenSim 被用于全球上百个生物力学实验室的运动研究，并拥有一个活跃的开发者社区来不断完善其功能。

但是，OpenSim生成的模型中默认不带有皮肤，因此需要我们去手动添加。

## 2. 示例(Body)部分
编译Opensim骨骼模型的文件为.osim格式，其写法类似于前端的HTML语言的xml语言

[https://github.com/user-attachments/assets/82bca19b-c789-4b60-931b-3766588aa9c3](https://github.com/user-attachments/assets/82bca19b-c789-4b60-931b-3766588aa9c3)

这里拿定义右大臂骨骼模型的代码为例：

```xml
<Body name="humerus_r">
    <!--The geometry used to display the axes of this Frame.-->
    <FrameGeometry name="frame_geometry">
        <!--Path to a Component that satisfies the Socket 'frame' of type Frame.-->
        <socket_frame>..</socket_frame>
        <!--Scale factors in X, Y, Z directions respectively.-->
        <scale_factors>0.20000000000000001 0.20000000000000001 0.20000000000000001</scale_factors>
    </FrameGeometry>
    <!--List of geometry attached to this Frame. Note, the geometry are treated as fixed to the frame and they share the transform of the frame when visualized-->
    <attached_geometry>
        <Mesh name="humerus_r_geom_1">
            <!--Path to a Component that satisfies the Socket 'frame' of type Frame.-->
            <socket_frame>..</socket_frame>
            <!--Scale factors in X, Y, Z directions respectively.-->
            <scale_factors>1.0124521312654093 1.0124521312654093 1.0124521312654093</scale_factors>
            <!--Default appearance attributes for this Geometry-->
            <Appearance>
                <!--Flag indicating whether the associated Geometry is visible or hidden.-->
                <visible>true</visible>
                <!--The opacity used to display the geometry between 0:transparent, 1:opaque.-->
                <opacity>1</opacity>
                <!--The color, (red, green, blue), [0, 1], used to display the geometry. -->
                <color>1 1 1</color>
                <!--Visuals applied to surfaces associated with this Appearance.-->
                <SurfaceProperties>
                    <!--The representation (1:Points, 2:Wire, 3:Shaded) used to display the object.-->
                    <representation>3</representation>
                </SurfaceProperties>
            </Appearance>
            <!--Name of geometry file.-->
            <mesh_file>humerus_rv.vtp</mesh_file>
        </Mesh>
    </attached_geometry>
    <!--Set of wrap objects fixed to this body that GeometryPaths can wrap over.This property used to be a member of Body but was moved up with the introduction of Frames.-->
    <WrapObjectSet name="wrapobjectset">
        <objects />
        <groups />
    </WrapObjectSet>
    <!--The mass of the body (kg)-->
    <mass>1.6941896825604743</mass>
    <!--The location (Vec3) of the mass center in the body frame.-->
    <mass_center>0 -0.16655040049742237 0</mass_center>
    <!--The elements of the inertia tensor (Vec6) as [Ixx Iyy Izz Ixy Ixz Iyz] measured about the mass_center and not the body origin.-->
    <inertia>0.010207114500963559 0.0035211383608296349 0.011457157068761123 0 0 0</inertia>
</Body>
```

### 2.1 大臂模型由<Body>标签定义。
### 2.2 <FrameGeometry>为坐标轴可视化。
<scale_factors>为坐标轴缩放因子

<socket_frame>继承父级坐标系变换(这个可以不管)

### 2.3 <attached_geometry>的几何可视化。
<Mesh>用来加载模型的三维网格

<socket_frame>表示该组件继承父框架坐标系

<scale_factors>为模型在XYZ轴的缩放系数，1 1 1为初始大小

#### 2.3.1 <Appearance>定义外观属性
<visible>定义模型是否可见

<opacity>为模型透明度，1为完全不透明

<color>定义颜色，RGB白色显示（取值范围0-1）

<representation>为渲染模式，可不管

#### 2.3.2 <mesh_file>定义模型文件路径
### 2.4 <WrapObjectSet>定义动力学交互组件
这个不怎么重要，一般都是附加一个球，椭圆啥的，跳过

### 2.5 剩余部分
<mass>定义质量

<mass_center>定义质心在局部坐标系中的坐标

<inertia>定义惯性张量（Ixx,Iyy,Izz,Ixy,Ixz,Iyz）

## 3. 演示(Body)部分

```xml
<!-- Right Upper Arm Skin Model -->
<Body name="skin_humerus_r">
    <mass>1.6941896825604743</mass>
    <!--The location (Vec3) of the mass center in the body frame.-->
    <mass_center>0 0 0</mass_center>
    <!--The elements of the inertia tensor (Vec6) as [Ixx Iyy Izz Ixy Ixz Iyz] measured about the mass_center and not the body origin.-->
    <inertia>0.010207114500963559 0.0035211383608296349 0.01 0 0 0</inertia>
    <Position>0  0  0</Position>
    <FrameGeometry name="frame_geometry">
        <!--Path to a Component that satisfies the Socket 'frame' of type Frame.-->
        <socket_frame>..</socket_frame>
        <!--Scale factors in X, Y, Z directions respectively.-->
        <scale_factors>0.30000000000000001 0.30000000000000001 0.30000000000000001</scale_factors>
    </FrameGeometry>
    
    <attached_geometry>
        <Mesh name="skin_humerus_right">
            <!--Path to a Component that satisfies the Socket 'frame' of type Frame.-->
            <socket_frame>..</socket_frame>
            <!--Scale factors in X, Y, Z directions respectively.-->
            <scale_factors>1.0124521312654093 1.0124521312654093 1.0124521312654093</scale_factors>
            <!--Default appearance attributes for this Geometry-->
            <Appearance>
                <!--Flag indicating whether the associated Geometry is visible or hidden.-->
                <visible>true</visible>
                <!--The opacity used to display the geometry between 0:transparent, 1:opaque.-->
                <opacity>1</opacity>
                <!--The color, (red, green, blue), [0, 1], used to display the geometry. -->
                <color>1 1 1</color>
                <!--Visuals applied to surfaces associated with this Appearance.-->
                <SurfaceProperties>
                    <!--The representation (1:Points, 2:Wire, 3:Shaded) used to display the object.-->
                    <representation>3</representation>
                </SurfaceProperties>
            </Appearance>
            <!--Name of geometry file.-->
            <mesh_file>D:\GaitCode\pose2sim-main\Pose2Sim\Demo_SinglePerson\parts\back-arm.stl</mesh_file>
        </Mesh>
    </attached_geometry>
</Body>
```
其中的<mesh_file>为一个大臂皮肤模型的路径(.STL)格式，在Blender中长这样：

[https://github.com/user-attachments/assets/2ba2b4bc-d8a1-4cb7-8081-202eada038d2](https://github.com/user-attachments/assets/2ba2b4bc-d8a1-4cb7-8081-202eada038d2)

重要提示：一定要让中心点为该模型的质心点，不然到最后无法让皮肤模型附加在上面。

## 4. 演示(Joint)部分
定义完Body部分，接着再来定义Joint部分，不然在Opensim导入文件时会报错。

```xml
<!-- Weld Joint for Right Upper Arm Skin Attachment -->
<WeldJoint name="humerus_r_skin_attachment">
    <socket_parent_frame>humerus_r_offset</socket_parent_frame>
    <socket_child_frame>humerus_r_skin_frame</socket_child_frame>

    <frames>
        <PhysicalOffsetFrame name="humerus_r_offset">
            <FrameGeometry name="frame_geometry">
                <socket_frame>...</socket_frame>
                <scale_factors>0.2 0.2 0.2</scale_factors>
            </FrameGeometry>
            <socket_parent>/bodyset/humerus_r</socket_parent>
            <translation>0 -0.15655040049742237 0</translation>
            <orientation>0 0 0</orientation>
        </PhysicalOffsetFrame>

        <PhysicalOffsetFrame name="humerus_r_skin_frame">
            <FrameGeometry name="frame_geometry">
                <socket_frame>...</socket_frame>
                <scale_factors>1 1 1</scale_factors>
            </FrameGeometry>
            <socket_parent>/bodyset/skin_humerus_r</socket_parent>
            <translation>0 0 0</translation>
            <orientation>0 0 0</orientation>
        </PhysicalOffsetFrame>
    </frames>
</WeldJoint>
```

这里定义了一个WeldJoint，用于将右大臂皮肤（skin_humerus_r）刚性地连接到相应的骨骼结构（humerus_r）上。焊接关节完全消除了两个刚体之间的任何相对运动，这正好符合我们对皮肤附着的期望。

### 4.1 关节配置
<socket_parent_frame>定义了父骨骼的偏移坐标系

<socket_child_frame>定义了子皮肤组件的坐标系

在此阶段，存在两个物理偏移坐标系，必须在下面定义

### 4.2 <frames>部分
本节定义了骨骼侧和皮肤侧的坐标系。

#### 4.2.1 <PhysicalOffsetFrame name="humerus_r_offset">
<socket_parent>指定了父组件（右大臂骨骼）

<translation>定义了在X、Y、Z轴上的位置；这里表示在骨骼局部坐标系中沿负Y方向偏移约0.157米

<orientation>定义了旋转，在这种情况下设置为无旋转

#### 4.2.2 <PhysicalOffsetFrame name="humerus_r_skin_frame">
这个框架与上一个几乎相同，其中<socket_parent>指定了右大臂皮肤。它与皮肤组件的原始坐标系完全对齐。

最终结果是，当骨骼（humerus_r）移动时，焊接关节强制humerus_r_offset和humerus_r_skin_frame重合，从而驱动皮肤组件（skin_humerus_r）跟随骨骼的运动。

## 5. Demonstration (Visualization)

![image](https://github.com/user-attachments/assets/21f227cf-bbf7-4c4c-9c30-def094c584af)

https://github.com/user-attachments/assets/f68a5c74-fbbd-40e3-8eb6-f6828acebcc4
