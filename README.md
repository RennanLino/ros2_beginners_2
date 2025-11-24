# 🚀 **ROS 2 Beginners Level 2: Modelagem e Simulação de Robôs**

Este projeto acompanha o curso **ROS Beginners Level 2** e documenta passo a passo a evolução de um robô desde a modelagem em **URDF/Xacro**, visualização no **RViz2**, simulação no **Gazebo** e integração via **bridge ROS ↔ Gazebo**.

---

## 📚 **Sumário**

* [Visão Geral](#-visão-geral)
* [Linha do Tempo de Aprendizado](#-linha-do-tempo-de-aprendizado-do-simples-ao-avançado)
* [Estrutura do Projeto](#-estrutura-do-projeto-pastas-e-arquivos)
* [Principais Tags URDF e Recursos Xacro](#-principais-tags-urdf-e-recursos-xacro)
* [Exportando o Modelo para o Gazebo](#-exportando-o-modelo-para-o-gazebo)
* [Bridge ROS ↔ Gazebo](#-bridge-ros--gazebo-tópicos-e-serviços)
* [RViz2](#-rviz2-salvamento-de-configurações-e-visualização)
* [Launch Files](#-launch-files-bringup-da-simulação-e-checagem-no-rviz2)
* [Pré-requisitos e Build](#-pré-requisitos-instalação-e-build)
* [Como Usar](#-como-usar-comandos-principais)
* [Dicas e Boas Práticas](#-dicas-e-boas-práticas)
* [Próximos Passos](#-próximos-passos)

---

## 🔎 **Visão Geral**

Este repositório contém:

* A descrição completa do robô em **Xacro/URDF**
* Sensores (ex.: câmera) e propriedades físicas para simulação
* Launch files para RViz2 e Gazebo
* Uma **bridge flexível** integrando tópicos/serviços ROS 2 com o Gazebo

---

## 🧭 **Linha do Tempo de Aprendizado (do simples ao avançado)**

### **1) Conceitos básicos de URDF/Xacro**

* Modularização em arquivos reutilizáveis
* Criação de `link` e `joint`

### **2) Propriedades visuais e materiais**

* Uso de geometrias básicas
* Centralização em `common_properties.xacro`

### **3) Propriedades físicas**

* Uso de `collision` e `inertial`
* Macros para cálculos de inércia

### **4) Módulos do robô**

* Base móvel
* Braço robótico
* Câmera

### **5) Montagem final**

* `my_robot.xacro` une todos os módulos

### **6) Visualização no RViz2**

* Publicação de `robot_description`
* Uso de configuração `.rviz`

### **7) Simulação no Gazebo**

* Arquivos `*_gazebo.xacro`
* Launch com mundo de teste

### **8) Bridge ROS ↔ Gazebo**

* Mapeamentos YAML de tópicos e serviços

---

## 🗂️ **Estrutura do Projeto (pastas e arquivos)**

### **📦 my_robot_description**

```
- CMakeLists.txt
- package.xml
- launch/display.launch.xml
- rviz/urdf_config.rviz
- urdf/
  ├── common_properties.xacro
  ├── mobile_base.xacro
  ├── arm.xacro
  ├── camera.xacro
  ├── mobile_base_gazebo.xacro
  ├── arm_gazebo.xacro
  └── my_robot.xacro
```

### **📦 my_robot_bringup**

```
- CMakeLists.txt
- package.xml
- launch/my_robot_gazebo.launch.xml
- config/gazebo_bridge.yaml
- worlds/test_world.sdf
```

---

## 🧩 **Principais Tags URDF e Recursos Xacro**

### **Tags essenciais**

* `robot`
* `link`
* `joint`
* `visual`
* `collision`
* `inertial`

### **Recursos Xacro**

* `xacro:macro`
* `xacro:include`
* `xacro:property`

### **Exemplo mínimo**

```xml
<link name="link_exemplo">
  <visual>
    <geometry><box size="0.2 0.2 0.2"/></geometry>
    <material name="Grey"/>
  </visual>

  <collision>
    <geometry><box size="0.2 0.2 0.2"/></geometry>
  </collision>

  <inertial>
    <mass value="1.0"/>
    <inertia ixx="..." iyy="..." izz="..." ixy="0" ixz="0" iyz="0"/>
  </inertial>
</link>
```

---

## 🏭 **Exportando o Modelo para o Gazebo**

* Arquivos `*_gazebo.xacro` adicionam **plugins**, fricção, sensores etc.
* O launch `my_robot_gazebo.launch.xml`:

  * Processa o Xacro
  * Injeta o robô no mundo `.sdf`
  * Inicia a bridge

---

## 🔄 **Bridge ROS ↔ Gazebo (tópicos e serviços)**

A bridge usa o arquivo YAML:

```
my_robot_bringup/config/gazebo_bridge.yaml
```

### **Exemplos de mapeamento**

* Câmera: `gazebo.msgs.Image` ↔ `sensor_msgs/msg/Image`
* CameraInfo
* JointState

**Benefício:** você interage apenas com tópicos ROS 2.

---

## 👁️ **RViz2: Pré-configuração e visualização**

* Arquivo pré-configurado: `rviz/urdf_config.rviz`
* Para salvar novas configurações: **File → Save Config As…**
* O launch `display.launch.xml` abre automaticamente o RViz2 com o robô carregado

---

## 🚀 **Launch Files: bringup da simulação e checagem no RViz2**

### **Visualizar o modelo no RViz2 (sem simulação):**

```bash
ros2 launch my_robot_description display.launch.xml
```

### **Simulação no Gazebo:**

```bash
ros2 launch my_robot_bringup my_robot_gazebo.launch.xml
```

---

## 🛠️ **Pré-requisitos, instalação e build**

### **Dependências**

* ROS 2 (Humble+)
* Gazebo ou Gazebo Sim
* RViz2
* `colcon` e `vcs`
* `ros_gz_bridge` (se usando Ignition/Gazebo Sim)

### **Build do workspace**

```bash
colcon build --symlink-install
source install/setup.bash
```

---

## ▶️ **Como usar (comandos principais)**

### **Visualizar no RViz2:**

```bash
ros2 launch my_robot_description display.launch.xml
```

### **Simular no Gazebo:**

```bash
ros2 launch my_robot_bringup my_robot_gazebo.launch.xml
```

### **Listar tópicos e serviços:**

```bash
ros2 topic list
ros2 service list
```

### **Assinar câmera (exemplo):**

```bash
ros2 topic echo /camera/image
```

---

## 💡 **Dicas e Boas Práticas**

* Separe descrição URDF/Xacro das extensões do simulador
* Reutilize propriedades comuns em `common_properties.xacro`
* Use launchs dedicados
* Versione arquivos `.rviz`
* Documente claramente a bridge YAML
