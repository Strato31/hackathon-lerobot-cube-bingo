# 🤖 Hackathon DEEL France–Québec

Welcome to the DEEL France–Québec Hackathon repository 🎉
Here you’ll find everything you need to get started: objectives, rules, tips, and resources to make the most out of this exciting challenge.

## 🎯 Task Overview

The challenge: train a robot to pick up cubes and place them on a wooden board according to the instructions on a card.

![](assets/example.gif)

Sounds simple? In practice, it’s a tough robotics + learning task! Success depends on how strategically you build datasets, train models, and plan your approach.

### 🏆 Evaluation Rules & Scoring

Your robot will be evaluated over 5 levels of increasing difficulty.
Each level has 5 trials. You must succeed on at least 3/5 trials to unlock the next level.

**Level 1** – Basic Grasping

Task: Pick up a cube and place it in the middle of the board in ≤ 20s.

Scoring: 5 points per success (+5 bonus if the cube is caught on the first attempt).

**Level 2** – Precise Placement

Task: Pick up a cube and place it in the correct location on the board in ≤ 20s.

Scoring: 10 points per success (+5 bonus for first-attempt catches).

**Level 3** – Two-Cube Challenge

Task: Pick and place 2 cubes correctly in ≤ 30s.

Scoring: 30 points per success.

> [!NOTE]
> 💡 Adding the second cube too early in your dataset may make Levels 1 & 2 harder.

**Level 4** – Three-Cube Challenge

Task: Pick and place 3 cubes correctly in ≤ 30s.

Scoring: 40 points per success.

**Level 5** – Vision + Reasoning

Task: Pick and place 3 cubes in ≤ 30s without being told positions explicitly (the robot must read and interpret the card).

Scoring: 50 points per success.

> [!WARNING]
> ⚠️ Reality check: With only 2 days, even reaching Level 2 is a big success!

### 🗓️ Schedule

**Day 1**

09:00–09:15: Hackathon intro

09:15–09:45: Team meet-up & strategy discussion

09:45: 🟢 Official start!

11:00 (recommended): Run first training tests to verify dataset format, logging, and checkpoints.

12:00 (recommended): Test inference from early checkpoints.

18:00: First evaluation attempt (even with partial training).

**Day 2**

09:00: Evaluation with overnight training results

16:30: Final evaluations & team presentations (strategies, results, lessons learned)

17:30: Clean-up and prep for MobiliT.AI poster session

### 🔍 Tips & Recommendations

Plan your recording setup: Each team gets 2 external cameras. Place them wisely (wrist, top, side view, etc.). Name them clearly when recording datasets.

Record plenty of data: Rotate who records to get variety. Consistency matters!

Mark positions: If you set up a controlled environment, mark object positions (objects may be moved overnight).

**Robustness vs. simplicity**:

- Controlled environment = easier early progress.
- Varied environments = robustness but needs more data.
- Curriculum strategy: Start with lots of Level 1 data, then gradually add Level 2, Level 3, etc. However, this is not an obligation. If you and your teammates want to win all or nothing we respect that!

> [!NOTE]
>💡 Check out the [dataset guidelines](#guidelines-for-collecting-data) for more details.

### ⏱️ Training Time Examples

- ACT: 150 episodes, 60k steps, RTX3090, batchsize 16 → ~6 hours
- SmolVLA: ~2k steps → TODO
- Pi0.5: ~2k steps → TODO

> [!NOTE]
> ⚠️ Loss curves may not always reflect real-world performance. Save checkpoints often and test on the robot!

### ✅ Key Takeaways

- Think strategically: scoring is incremental, don’t rush for Level 5.
- Save checkpoints and test early & often.
- Teamwork matters as much as models: rotate recording, share insights.
- Remember: the goal is to learn, experiment, and have fun 🎉

## ⚙️ Setup Instructions

This guide explains how to prepare your environment for the DEEL France–Québec Hackathon.  
We cover three main scenarios:
1. Recording data on a **Windows machine**.
2. Training models on **DEEL’s cluster machines**.
3. Running **inference on Windows** with your trained checkpoints.

### 📂 Source Code

For the hackathon, we provide a dedicated fork of Hugging Face’s [LeRobot](https://github.com/huggingface/lerobot/tree/main) repository.

👉 Use the [custom repository](https://github.com/deel-ai/lerobot-hackathon) as your codebase.

You can distinguish at least two installations settings. The minimum installation on Windows for recording the data and for inference, and the setup for training a model.
We provide here the instructions for the training supposing you will use one of the machines prodvided to you on the DEEL's cluster. If you want to do it in your own environment some adaptation may be needed.

> [!NOTE]
> Training can be tested independently of data recording, since we provide you with a ready-to-use dataset.

> [!WARNING]
> We advise to seriously read the [dataset guidelines section](#guidelines-for-collecting-data) and this [blog post](https://huggingface.co/blog/sherryxychen/train-act-on-so-101) before recording your dataset.

### 🖥️ Setup for Recording on Windows

1. Install FFmpeg

Open PowerShell and run:
```shell
winget install ffmpeg
```

2. Clone the Repository

```shell
git clone git@github.com:deel-ai/lerobot-hackathon.git
cd lerobot-hackathon
```

3. Create and Activate Conda Environment

```shell
conda create -y -n lerobot python=3.10
conda activate lerobot
```

4. Install Dependencies

```shell
pip install -e .
pip install -e ".[feetech]"
```

#### 🔧 Robot Calibration

Calibration ensures that leader and follower arms map correctly.  
A well-calibrated robot allows a model trained on one setup to generalize to another.

1. Connect the Arms
- Power both arms.
- Use **USB-C → USB-A** cables to connect arms to your PC.
- Always use the same USB ports for consistency.

Find the port names:

```shell
lerobot-find-port
```

Example output:

```shell
Finding all available ports for the MotorBus.
['COM4', 'COM5']
Remove the USB cable from your MotorsBus and press Enter when done.

[...Disconnect corresponding leader or follower arm and press Enter...]

The port of this MotorsBus is 'COM4'
Reconnect the USB cable.
```

In our example, if you disconnected the leader arm, then the leader arm is on `'COM4'` and the follower `'COM5'`.

Remember it, as it will have its importance when you will operate.

2. Calibrate the Follower Arm

```shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port='COM5' \ # <- The port of your robot
    --robot.id=follower_idontheetiquette  \ # <- Give the robot a unique name
    --robot.calibration_dir="path\to\lerobot-hackathon\calibration\robots\so101_follower"
```

> [!NOTE]
> Using Powershell you should replace the " \ " with "`", and using the shell with "^"

For the calibration itself, the easiest is to follow the video in the dedicated section in this [tutorial](https://huggingface.co/docs/lerobot/so101#calibrate).

Your file will be saved in your calibration dir with the name "follower_idontheetiquette.json". 
This is interesting as once a calibration has been properly done you can share it with member of your teams without the need of doing the calibration again.

3. Calibrate the Leader Arm

```shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port='COM4' # <- The port of your robot
    --teleop.id=leader_idontheetiquette # <- Give the robot a unique name
    --teleop.calibration_dir="path\to\lerobot-hackathon\calibration\teleoperators\so101_leader"
```

> [!NOTE]
> The `--robot` flag is for the **follower** and `--teleop` for the **leader** arm.

4. Test Calibration

While having your hands on the **leader** arm:

```shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port="COM5" \
    --robot.id="follower_f0" \
    --robot.calibration_dir="path\to\lerobot-hackathon\calibration\robots\so101_follower" \
    --teleop.type=so101_leader \
    --teleop.port="COM4" \
    --teleop.id="leader_l0" \
    --teleop.calibration_dir="path\to\lerobot-hackathon\calibration\teleoperators\so101_leader" \
    --display_data=true
```

A `rerun.io` window should open. The follower arm must match the leader precisely. In case of lags, trying killing unnecessary processes.  
If it is still not working → **redo calibration**.

#### 🎥 Adding Cameras

1. Plug cameras into your computer.

A dock might be needed to have enough port and to have your computer charging.

2. Identify them:

```shell
lerobot-find-cameras opencv
```
Example output:
```shell
--- Detected Cameras ---
Camera #0:
  Name: OpenCV Camera @ 0
  Type: OpenCV
  Id: 0
  Backend api: AVFOUNDATION
  Default stream profile:
    Format: 16.0
    Width: 1920
    Height: 1080
    Fps: 15.0
--------------------
(more cameras ...)
```

> [!NOTE]
> This identifier might change if you reboot your computer or re-plug your camera, a behavior mostly dependant on your operating system.

3. Match IDs with physical placement by checking saved images in:

```shell
repository_dir/outputs/captured_images
```
Let's say that `opencv_0.png` is a camera positioned on the left of our robot and `opencv_2.png` correspond to a camera positioned
in front of it.

4. Example teleoperation with two cameras:
```shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port="COM5" \
    --robot.id="follower_f0" \
    --robot.calibration_dir="path\to\lerobot-hackathon\calibration\robots\so101_follower" \
    --robot.cameras="{ left: {type: opencv, index_or_path: 0, width: 1280, height: 720, fps: 30},  front: {type: opencv, index_or_path: 2, width: 1280, height: 720, fps: 30}}" ^ # <-- Your setting
    --teleop.type=so101_leader \
    --teleop.port="COM4" \
    --teleop.id="leader_l0" \
    --teleop.calibration_dir="path\to\lerobot-hackathon\calibration\teleoperators\so101_leader" \
    --display_data=true
```

You should now "see" what your robots is seeing in the `rerun.io` window. It will be useful to use that vision before actually recording data to place them as you wish.

#### 🎙️ Recording Data
Once you are happy with your installation setting you are now ready to record data. 

Each team has a Hugging Face dataset repo:  
`DEEL-AI/Hackathon_TeamXX`

**Keyboard Shortcuts During Recording**

Before running the recording command, it’s useful to know the available shortcuts:

- Redo a recording
    If you consider the current attempt low-quality, press ← (left arrow).
    - This gives you time to reset the environment and place the robot back in its initial position.
    - Once ready, press → (right arrow) to start recording again.

- Save a recording
    When you’re satisfied with an episode and have placed the robot in its final position, press → (right arrow).
    - This saves the episode.
    - You then have time to reset the environment before pressing → (right arrow) again to start the next episode.

> [!NOTE]
> A practical shortcut: if you’re happy with a recording, simply double-press the right arrow. The saving process takes long enough to let you reset the environment before the next episode starts.

The command to record:

```shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port='COM5' \
    --robot.id=follower_f0 \
    --robot.calibration_dir="path\to\lerobot-hackathon\calibration\robots\so101_follower" \
    --teleop.type=so101_leader \
    --teleop.port='COM4' \
    --teleop.id=leader_l0 \
    --teleop.calibration_dir="path\to\lerobot-hackathon\calibration\teleoperators\so101_leader" \
    --display_data=true \
    --robot.cameras="{ left: {type: opencv, index_or_path: 0, width: 1280, height: 720, fps: 30},  front: {type: opencv, index_or_path: 2, width: 1280, height: 720, fps: 30}}"  \ # <-- Change with your setting
    --dataset.root='path_to_locally_save_the_ds' \
    --dataset.repo_id='DEEL-AI/Hackathon_TeamXX' \
    --dataset.num_episodes=25 \ # Number of episodes you will record at once
    --dataset.single_task="Pick and place one green cube at the middle of the 3 by 3 grid." \ # <-- can be adapted but most follow the guidelines in the dataset section
    --dataset.push_to_hub=True \
    --resume=false \ # <-- Set to true once it has been initialized
```

> [!WARNING]
> Change `--resume` to `true` **after your first recording**.

> [!NOTE]
> You can change the `--dataset.single_task` to change the command prompt. For example "Pick and place one green cube at the top-left of the 3 by 3 grid.""

**BEFORE RECORDING**, take a look at the [dataset guidelines](#guidelines-for-collecting-data).

### 🖥️ Setup for Training with DEEL's machines

1. Create environment:
```shell
conda create -y -n lerobot python=3.10
conda activate lerobot
```

2. Install FFmpeg:
```shell
conda install ffmpeg=7.1.1 -c conda-forge
```

Checks that when you do:
```shell
which ffmpeg
```

Outputs looks like:
```shell
/home/lucas.hervier/.conda/envs/lerobot/bin/ffmpeg
```

And that you see `libsvtav1` in the list of the output of the following command:
```shell
ffmpeg -encoders
```

3. Clone & install repo:
```shell
git clone git@github.com:deel-ai/lerobot-hackathon.git
cd lerobot-hackathon
pip install -e .
```

4. Authenticate:
```shell
hf auth login
wandb login
```

> [!NOTE]
> Make sure that you use a token with the right permissions

5. Download dataset:
```shell
hf download DEEL-AI/Hackathon_TeamXX --repo-type dataset
```

#### Training with ACT

```shell
CUDA_VISIBLE_DEVICE="id_of_your_gpu" lerobot-train \
  --dataset.repo_id=DEEL-AI/Hackathon_TeamXX \
  --policy.type=act \
  --output_dir=/output_dir_with_space/train/act_so101_test \
  --job_name=act_so101_test \
  --policy.device=cuda \
  --policy.push_to_hub=false \
  --batch_size=64 \
  --save_freq=100 \
  --steps=200 \
  --wandb.enable=true \
  --wandb.disable_artifact=true
```

As a test you can run the following script with `dataset.repo_id=DEEL-AI/Hackathon_Team0Z`. (Don't forget to download it with `hf download DEEL-AI/Hackathon_Team0Z --repo-type dataset`)

For this test I voluntarily set a low number of steps and a saving frequency that is low. **However, having such a low frequency will fill up space quickly so be mindful of this parameter**.

ACT come with default values for `steps` and `save_freq`. It is up to you to check if the are those you want to use.

If you want to resume a training:

```shell
lerobot-train \
  --config_path=/output_dir_with_space/train/outputs/act_so101_test/checkpoints/last/pretrained_model/train_config.json \
  --resume=true \
  --policy.device=cuda \
  --policy.push_to_hub=false \
  --steps=400 \
  --wandb.enable=true \
  --wandb.disable_artifact=true
```

> [!WARNING]
> Here `--steps` is not the number of additional steps you want to do. Is the number of steps it must reach from the CKPT step you provided.


#### Training with SmolVLA

Additionnally to finetune SmolVLA:

```shell
pip install -e ".[smolvla]"
```

Finally **check your pytorch version** and use `set_cuda_version` with matching distributions.

Then:

```shell
lerobot-train \
  --policy.path=cijerezg/smolvla-test \
  --dataset.repo_id=DEEL-AI/Hackathon_TeamXX \
  --output_dir=/output_dir_with_space/train/smolvla_so101_test \
  --job_name=smolvla_so101_test \
  --policy.device=cuda \
  --policy.push_to_hub=false \
  --batch_size=64 \
  --steps=400 \
  --save_freq=200 \
  --wandb.enable=true \
  --wandb.disable_artifact=true
```

As a test you can run the following script with `dataset.repo_id=DEEL-AI/Hackathon_Team0Z`

To resume training it is the same as for ACT:

```shell
lerobot-train \
  --config_path=/output_dir_with_space/train/outputs/smolvla_so101_test/checkpoints/last/pretrained_model/train_config.json \
  --resume=true \
  --policy.device=cuda \
  --policy.push_to_hub=false \
  --steps=400 \
  --wandb.enable=true \
  --wandb.disable_artifact=true
```

### Inference on Windows

To run inference on your PC, you first need to get your trained checkpoints.
If you want to get them from the DEEL's machines you can use scp:

```shell
scp -r username@deelXX://output_dir_with_space/train/act_so101_test/checkpoints/last C:\path\to\outputs\train\act_so101_test\checkpoints
```

Then, if you have the environment as build in the [recording section](#setup-for-recording-with-a-windows-machine) you can run:

```shell
lerobot-record  ^
    --robot.type=so101_follower \
    --robot.port='COM5' \
    --robot.id=follower_f0 \
    --robot.calibration_dir="path\to\lerobot-hackathon\calibration\robots\so101_follower" \
    --robot.cameras="{ left: {type: opencv, index_or_path: 0, width: 1280, height: 720, fps: 30},  front: {type: opencv, index_or_path: 2, width: 1280, height: 720, fps: 30}}"  \ # <-- Change with your setting
    --teleop.type=so101_leader \
    --teleop.port='COM4' \
    --teleop.id=leader_l0 \
    --teleop.calibration_dir="path\to\lerobot-hackathon\calibration\teleoperators\so101_leader" \
    --display_data=true \
    --dataset.single_task="Pick and place one green cube at the middle of the 3 by 3 grid." ^
    --dataset.root='./eval_hackathon_9_cubes_v1' ^
    --dataset.repo_id='DEEL-AI/eval_Hackathon_TeamXX' ^
    --dataset.push_to_hub=false ^
    --policy.path="C:\path\to\outputs\train\act_so101_test\checkpoints\last\pretrained_model"
```

You don’t need to include the teleop arguments if you prefer not to. However, adding them allows you to press ← (left arrow) during inference to temporarily take manual control of the robot and reset it, before pressing → (right arrow) to continue to the next episode.

> [!NOTE]
> You can modify the --dataset.single_task flag to change the command prompt. That said, we recommend using the exact same commands as those in your dataset to ensure consistency.

The keyboard shortcuts behave the same way as during recording, except you won’t need to teleoperate—the robot will autonomously execute episodes.
For testing, we provide a sample policy that you can run before training your own. Its behavior may be erratic, but it’s useful for verifying that inference is working correctly.

✅ You are now fully set up and ready to record, train, and run inference!

## Datasets

### Guidelines for collecting data

From this [HF blog post](https://huggingface.co/blog/lerobot-datasets):

<img width="1414" height="2000" alt="image" src="https://github.com/user-attachments/assets/3288375f-621b-4865-9a45-497b296d28a9" />


### Available on the Hugging Face Hub

Browse all datasets: [HF Datasets](https://huggingface.co/datasets?other=LeRobot&sort=trending)

**Visualize a LeRobot Dataset**
Use the interactive tool: [HF Robot Viz Space](https://huggingface.co/spaces/lerobot/visualize_dataset)

### Streaming

Official documentation: [HF Docs on Dataset Streaming](https://huggingface.co/docs/datasets/stream)

**Streaming LeRobot Dataset**
TODO

## TODO

### Logistics
- [ ] At least 1 core member with a full setup per team (??/8)
- [ ] Make cross-validation of produced code
- [ ] Build the teams
- [x] 4 Developers
- [x] 4 Testers

### Hardware
- [x] Print all the Leader (8/8)
- [ ] Print all the Follower (6/8)
- [ ] Prepare a notice to be able to calibrate arms correctly
- [x] Prepare a notice to set machines to record data
- [ ] Print parts for the Camera
- [ ] Prepare the WoodBoards (2/7)

### Datasets
- [x] Define the best way to handle the generated datasets (per team)
- [x] Make utils code to ease the dataset manipulation

### Training
- [x] Make utils code to train using ACT on DEEL's machine
- [x] Make utils code to finetune SmolVLA
- [x] Prepare a notice to train on a custom dataset with the defined dataset's strategy 
