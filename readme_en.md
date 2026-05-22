<div align="center">
  <h1>Go2_piper_mjlab</h1>
  <img src="images/2026-05-22 13-51-39.gif">
    <p>
      <a href="./readme.md"><kbd>Chinese</kbd></a>
      <strong><kbd>English</kbd></strong>
    </p>
</div>

# Project Description

This project provides a reinforcement learning implementation for the Unitree Go2 + Agilex Piper robot in mjlab. It combines the Unitree Go2 series with the Agilex arm series.

# Quick Start

## Task ID List

```bash
Mjlab-Velocity-Flat-Go2arm
Mjlab-Velocity-Rough-Go2arm
```

## Environment Setup

Set up the uv environment:

```bash
uv sync
```

## Environment Check

Run an environment check without taking any actions:

```bash
uv run play Mjlab-Velocity-Flat-Go2arm \
  --agent zero \
  --viewer viser \
  --num-envs 1
```

Run an environment check with random actions:

```bash
uv run play Mjlab-Velocity-Flat-Go2arm \
  --agent random \
  --viewer viser \
  --num-envs 1
```

## Training

You can use wandb or tensorboard for logging. wandb is recommended for real-time visualization of the training process.

Training on flat terrain:

```bash
uv run train Mjlab-Velocity-Flat-Go2arm \
  --env.scene.num-envs 4096 \
  --agent.logger wandb
```

Training on rough terrain:

```bash
uv run train Mjlab-Velocity-Rough-Go2arm \
  --env.scene.num-envs 4096 \
  --agent.logger wandb
```

Resume training:

```bash
# Replace the load-checkpoint parameter with the path to a model file saved during training.
uv run train Mjlab-Velocity-Flat-Go2arm \
  --env.scene.num-envs 4096 \
  --agent.resume True \
  --agent.load-run RUN_DIRECTORY_NAME \
  --agent.load-checkpoint model_1000.pt \
  --agent.logger wandb
```

## Play a Trained Policy

When playing a policy, replace the checkpoint-file parameter with the path to a model file saved during training.

```bash
uv run play Mjlab-Velocity-Flat-Go2arm \
  --checkpoint-file /path/to/model.pt \
  --viewer viser \
  --num-envs 1
```

Visualization-only playback with terminations disabled:

```bash
uv run play Mjlab-Velocity-Flat-Go2arm \
  --checkpoint-file /path/to/model.pt \
  --viewer viser \
  --num-envs 1 \
  --no-terminations True
```

## sim2sim

MuJoCo can be used for sim2sim validation. Specify the checkpoint file path when running.

Specify task parameters:

```bash
uv run python deploy/simulation/sim2sim.py \
  --checkpoint /path/to/model.pt \
  --lin-vel-x 0.2 \
  --lin-vel-y 0.0 \
  --ang-vel-z 0.0 \
  --ee-x 0.48 \
  --ee-y 0.0 \
  --ee-z 0.36
```

Keyboard control:

```bash
uv run python deploy/simulation/sim2sim_keyboard.py \
  --checkpoint /path/to/model.pt
```

# Acknowledgments

This project is built on top of the [mjlab](https://github.com/mujocolab/mjlab) framework. Thanks to the authors and contributors of mjlab for open-sourcing this project for developers to use.

The reinforcement learning setup for the quadruped robot plus robotic arm references [Go2Arm_Lab](https://github.com/zzzJie-Robot/Go2Arm_Lab) and [Go2_ARX_mjlab](https://github.com/Czy213hd/Go2_ARX_mjlab).

# License

This repository is based on mjlab and keeps the original Apache-2.0 license. See LICENSE.

Third-party assets and code retain their original licenses. In particular, check the license files bundled with the Go2 and Piper assets before using them in commercial or redistributed projects.

If you use the underlying mjlab framework in research, please also cite the original mjlab project.
