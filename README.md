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

## Source Code

For the hackathon, we provide a dedicated fork of Hugging Face’s [LeRobot](https://github.com/huggingface/lerobot/tree/main) repository.
This fork is based on the v0.3.3 release, to which we applied several bug fixes to ensure a consistent and reliable setup for all participants.

👉 Please make sure to use this [custom repository](https://github.com/deel-ai/lerobot-hackathon) as your codebase, following the instructions below.

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
- [x] 4 Developers
- [x] 4 Testers

### Hardware
- [x] Print all the Leader (8/8)
- [ ] Print all the Follower (6/8)
- [ ] Prepare a notice to be able to calibrate arms correctly
- [ ] Prepare a notice to set machines to record data
- [ ] Print parts for the Camera
- [ ] Prepare the WoodBoards

### Datasets
- [ ] Define the best way to handle the generated datasets (per team)
- [ ] Make utils code to ease the dataset manipulation

### Training
- [ ] Make utils code to train using ACT on DEEL's machine
- [ ] Make utils code to train using ACT with other machines
- [ ] Make utils code to finetune SmolVLA
- [ ] Prepare a notice to train on a custom dataset with the defined dataset's strategy 
