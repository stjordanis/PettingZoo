## Atari Environments



{% include bigtable.html group="atari/" cols=4 %}

The Atari environments are based off the [Arcade Learning Environment](https://github.com/mgbellemare/Arcade-Learning-Environment). This environment was instrumental in the development of modern reinforcement learning, and so we hope that our [multi-agent version](https://github.com/PettingZoo-Team/Multi-Agent-ALE) of it will be useful in the development of multi-agent reinforcement learning.

### Games overview

Most games are two player, with the exception of Warlords and a couple of Pong variations which are four player.

There are three types of games:

### Environment details

The ALE environment has been studied extensively and examined for various flaws and how to fix them.  

* Determinism: The Atari console is deterministic, and so agents can theoretically memorize precise sequences of actions that will maximize the end score. This is not ideal, so we encourage the use of [SuperSuit's](https://github.com/PettingZoo-Team/SuperSuit) `sticky_actions` wrapper (example below). This is the recommended approach of  *"Machado et al. (2018), "Revisiting the Arcade Learning Environment: Evaluation Protocols and Open Problems for General Agents"*
* Frame flickering: Atari games often do not render every sprite every frame due to hardware restrictions. Instead, sprites (such as the knights in Joust) are sometimes rendered every other frame or even (in Wizard of Wor) every 3 frames. The standard way of handling this is a frame stack of the previous 4 observations (see example below for implementation).

### Preprocessing

We encourage the use of the [supersuit](https://github.com/PettingZoo-Team/SuperSuit) library for preprocessing. This library can be installed with `pip install supersuit`.

Here is some example usage for the Atari preprocessing.

```python
from supersuit import resize, frame_skip, frame_stack, sticky_actions
from pettingzoo.atari import space_invaders_v0

env = space_invaders_v0.env()

# repeat_action_probability is set to 0.25 by default to fix the determinism issue
# set the seed parameter sticky_actions(env, repeat_action_probability=0.25, seed=0)
# for deterministic evaluation
env = sticky_actions(env, repeat_action_probability=0.25)

# downscale observation for faster processing
env = resize(env, (84, 84))

# to allow agent to see everything on the screen despite atari's flickering screen problem
env = frame_stack(env, 4)

# skip frames for faster processing and less control
# to be compatable with gym, do frame_skip(env, (2,5))
env = frame_skip(env, 4)
```

### Common Parameters

All the Atari environments have the following environment parameters:

```
<atari_game>.env(seed=None, obs_type='rgb_image', full_action_space=True, max_frames=100000)
```

`seed`: Set to specific value for deterministic, reproducible behavior.

`obs_type`:  default value of 'rgb_image' leads to (210, 160, 3) image pixel observations like you see as a human, 'grayscale_image' leads to a black and white (210, 160, 1) image, 'ram' leads to an observation of the 1024 bits that comprise the RAM of the atari console.

`full_action_space`:  The effective action space of the atari games is often smaller than the full space of 18 moves. Setting this to False shrinks the action space to this smaller space.

`max_frames`:  number of frames (a step for each agent) until game terminates


### Citation

If you use the Atari environments in your research please cite the following paper:

```
@Article{bellemare13arcade,
  author = { {Bellemare}, M.~G. and {Naddaf}, Y. and {Veness}, J. and {Bowling}, M.},
  title = {The Arcade Learning Environment: An Evaluation Platform for General Agents},
  journal = {Journal of Artificial Intelligence Research},
  year = "2013",
  month = "jun",
  volume = "47",
  pages = "253--279",
}
```
