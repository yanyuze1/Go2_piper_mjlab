<div align="center">
  <h1>Go2_piper_mjlab</h1>
    <p>
      <strong><kbd>中文</kbd></strong>
      <a href="./README_EN.md"><kbd>English</kbd></a>
    </p>
</div>

# 项目描述
本项目是Unitree Go2 + Agilex piper机器人在mjlab中的强化学习实现，是Unitree Go2系列和Agilex arm系列的结合。

# 快速开始
## 任务id列表：
```bash
Mjlab-Velocity-Flat-Go2arm
Mjlab-Velocity-Rough-Go2arm
```
## 环境配置
uv环境配置：
```bash
uv sync
```
## 环境检查
运行环境检查，不采取任何行为：
```bash
uv run play Mjlab-Velocity-Flat-Go2arm \
  --agent zero \
  --viewer viser \
  --num-envs 1
```
运行环境检查，采取随机执行动作：
```bash
uv run play Mjlab-Velocity-Flat-Go2arm \
  --agent random \
  --viewer viser \
  --num-envs 1
```
## 训练
可以使用wandb或tensorboard进行日志记录，推荐使用wandb来实时可视化训练过程。

平坦地形训练：
```bash
uv run train Mjlab-Velocity-Flat-Go2arm \
  --env.scene.num-envs 4096 \
  --agent.logger wandb
```
崎岖地形训练：
```bash
uv run train Mjlab-Velocity-Rough-Go2arm \
  --env.scene.num-envs 4096 \
  --agent.logger wandb
```
恢复训练：
```bash
# 运行时请将load-checkpoint参数替换为训练过程中保存的模型文件路径。
uv run train Mjlab-Velocity-Flat-Go2arm \
  --env.scene.num-envs 4096 \
  --agent.resume True \
  --agent.load-run RUN_DIRECTORY_NAME \
  --agent.load-checkpoint model_1000.pt \
  --agent.logger wandb
```
## 播放训练策略
播放策略时请将checkpoint-file参数替换为训练过程中保存的模型文件路径。
```bash
uv run play Mjlab-Velocity-Flat-Go2arm \
  --checkpoint-file /path/to/model.pt \
  --viewer viser \
  --num-envs 1
```
禁止终端的仅可视化播放：
```bash
uv run play Mjlab-Velocity-Flat-Go2arm \
  --checkpoint-file /path/to/model.pt \
  --viewer viser \
  --num-envs 1 \
  --no-terminations True
```
## sim2sim
可使用mujoco完成sim2sim验证。运行时需指定checkpoint文件路径。

指定任务参数：
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
键盘控制：
```bash
uv run python deploy/simulation/sim2sim_keyboard.py \
  --checkpoint /path/to/model.pt
```

# 致谢
该项目建立在[mjlab](https://github.com/mujocolab/mjlab)基础框架之上，感谢mjlab的作者和贡献者们将此项目开源提供给广大开发者进行使用。
机器狗+机械臂强化学习参考了[Go2Arm_Lab](https://github.com/zzzJie-Robot/Go2Arm_Lab)和[Go2_ARX_mjlab](https://github.com/Czy213hd/Go2_ARX_mjlab)

# License

This repository is based on mjlab and keeps the original Apache-2.0 license. See LICENSE.

Third-party assets and code retain their original licenses. In particular, check the license files bundled with the Go2 and ARX L5 assets before using them in commercial or redistributed projects.

If you use the underlying mjlab framework in research, please also cite the original mjlab project.
