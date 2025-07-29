

![dance1_subject2 motion](/dance_subject.gif)

## 1. LAFAN1 Retargeting Dataset(about AMASS motion retargeting you can check: [AMASS-Motion-Retargeting](https://github.com/XinLang2019/AMASS-Motion-Retargeting.git)

To make the motion of humanoid robots more natural, we retargeted [LAFAN1](https://github.com/ubisoft/ubisoft-laforge-animation-dataset) motion capture data to [Unitree](https://www.unitree.com/)'s humanoid robots, supporting three models: [H1, H1_2](https://www.unitree.com/h1), and [G1](https://www.unitree.com/g1). This retargeting was achieved through numerical optimization based on [Interaction Mesh](https://ieeexplore.ieee.org/document/6651585) and IK, considering end-effector pose constraints, as well as joint position and velocity constraints, to prevent foot slippage. It is important to note that the retargeting only accounted for kinematic constraints and did not include dynamic constraints or actuator limitations. As a result, the robot cannot perfectly execute the retargeted trajectories.

## 2. How to visualize robot trajectories in Isaacgym?

### Step 1: Create conda environment
```sh
# Step 1: Set up a Conda virtual environment
conda create -n lafan-data python=3.8
conda activate lafan-data
```
### Step 2: Install dependencies
Install Isaac Gym
   - Download and install Isaac Gym Preview 3 (Preview 2 will not work!) from https://developer.nvidia.com/isaac-gym
   - `cd isaacgym/python && pip install -e .`
   - Try running an example `cd examples && python 1080_balls_of_solitude.py`
   - For troubleshooting check docs `isaacgym/docs/index.html`)

```sh
pip install torch numpy argparse
```

## 3. run the script with parameters:
```sh
python issacgym_visualize.py --file_name dance1_subject2 --robot_type g1
```
- `robot_type` can choose: `g1`, `h1`, `h2`


## 4. convert data format

you can use this code to convert data format as `.pkl`, this data format can be used to train policy with [ASAP](https://github.com/LeCAR-Lab/ASAP.git)

```sh
python cvs_to_pkl.py
```
after running this order, you can get a file in `pkl_data/`, But you need to note that this code can only convert `g1 robot` data currently.If you want to use it to convert `h1 robot` data, you can modify it based on that.

#### pkl data description
```sh
data_dump[data_name]={
                "root_trans_offset": root_trans_all.cpu().detach().numpy(),
                "pose_aa": pose_aa.squeeze().cpu().detach().numpy(),   
                "dof": dof_pos_all.detach().cpu().numpy(), 
                "root_rot": root_rot_all.cpu().numpy(),
                "fps": 30
                }
```

## 4. Dataset Collection Description

This database stores the retargeted trajectories in CSV format. Each row in the CSV file corresponds to the original motion capture data for each frame, recording the configurations of all joints in the humanoid robot in the following order:

```txt
The Order of Configuration
G1: (30 FPS)
    root_joint(XYZ QXQYQZQW) 7vetor
    left_hip_pitch_joint
    left_hip_roll_joint
    left_hip_yaw_joint
    left_knee_joint
    left_ankle_pitch_joint
    left_ankle_roll_joint
    right_hip_pitch_joint
    right_hip_roll_joint
    right_hip_yaw_joint
    right_knee_joint
    right_ankle_pitch_joint
    right_ankle_roll_joint
    waist_yaw_joint
    waist_roll_joint
    waist_pitch_joint
    left_shoulder_pitch_joint
    left_shoulder_roll_joint
    left_shoulder_yaw_joint
    left_elbow_joint
    left_wrist_roll_joint  19
    left_wrist_pitch_joint
    left_wrist_yaw_joint   21
    right_shoulder_pitch_joint 22
    right_shoulder_roll_joint
    right_shoulder_yaw_joint
    right_elbow_joint         25
    right_wrist_roll_joint 26
    right_wrist_pitch_joint 34
    right_wrist_yaw_joint 35
H1_2: (30 FPS)
    root_joint(XYZQXQYQZQW)
    left_hip_yaw_joint
    left_hip_pitch_joint
    left_hip_roll_joint
    left_knee_joint
    left_ankle_pitch_joint
    left_ankle_roll_joint
    right_hip_yaw_joint
    right_hip_pitch_joint
    right_hip_roll_joint
    right_knee_joint
    right_ankle_pitch_joint
    right_ankle_roll_joint
    torso_joint
    left_shoulder_pitch_joint
    left_shoulder_roll_joint
    left_shoulder_yaw_joint
    left_elbow_joint
    left_wrist_roll_joint
    left_wrist_pitch_joint
    left_wrist_yaw_joint
    right_shoulder_pitch_joint
    right_shoulder_roll_joint
    right_shoulder_yaw_joint
    right_elbow_joint
    right_wrist_roll_joint
    right_wrist_pitch_joint
    right_wrist_yaw_joint
H1: (30 FPS)
    root_joint(XYZQXQYQZQW)
    left_hip_yaw_joint
    left_hip_roll_joint
    left_hip_pitch_joint
    left_knee_joint
    left_ankle_joint
    right_hip_yaw_joint
    right_hip_roll_joint
    right_hip_pitch_joint
    right_knee_joint
    right_ankle_joint
    torso_joint
    left_shoulder_pitch_joint
    left_shoulder_roll_joint
    left_shoulder_yaw_joint
    left_elbow_joint
    right_shoulder_pitch_joint
    right_shoulder_roll_joint
    right_shoulder_yaw_joint
    right_elbow_joint
```

