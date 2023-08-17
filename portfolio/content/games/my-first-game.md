+++
authors = "Jacob Cheney"
title = "The Shallows"
date = "2023-08-17"
description = "A top-down pixel-shmup dungeon crawler roguelite built in rust using the Bevy game engine"
tags = [
    "game",
    "graphics"
]
categories = [
    "gamdev",
]
wasm = "shallows"
+++

## Controls

Movement - WASD
Shooting - Mouse + Click
UI - Press esc to navigate "back" in any given ui state

---

This is a pre-pre-alpha demo version of a game I've been working on as a way to learn Rust on a more intricate level. The gameplay is very barebones right now. A single level with some enemies, once you kill them all - you win. 
 

## [Bevy](https://bevyengine.org/)

 Bevy is a really amazing project built by a ton of cool people. It's also one of few engines I've tried that fits all my requirements whenever I would want to sit down and "make a game". The amount of control and flexibility you get with an ECS framework is staggering. This also poses a cognitive challenge as well though. I'm mostly used to OOP but have come to prefer data oriented paradigms as the complexities of my projects grow in size. 

 Unfortunately, the game currently isn't open source as some of the assets I use(d) require me to project them when distributing. I'm in the process of trying to find alternative assets or make my own so I can potentially open source the project in the future. I don't have any solid plans on monetizing this project but that may change in the future.

 Making full use of bevy for sure requires some intimate knowlege of rust - especially generics. However, the open source nature of bevy makes it extraordinarily easy to just go into the source code and dig into the internals of how something works. You can't do that with other engines unless you (usually) pay a premium to access source code.

 The community is also really fast and adaptable as well. This game has been through 3 whole releases of the bevy engine (0.9->0.11.1) and each time I've managed to upgrade with extreme ease because you can easily point any depedencies towards a specific git branch if need be.

 However - there are some points of pain that took me time to get used to. 
 
 The asset management system is not fully matured yet and required me to use third party crates which took a bit of getting used to. However, [AssetsV2](https://github.com/bevyengine/bevy/pull/8624) should hit soon.
 
 Implementing physics was challenging since I ended up relying on [Rapier](https://rapier.rs/) for physics. Rapier is very good but I had to do some custom scheduling of systems to smoothly integrate everything.

 Animations took me the longest amount of time because I was just getting comfortable with rust generics around the time I needed a way to make animations... well ... generic. I decided to use [Leafwing Input Manager](https://github.com/Leafwing-Studios/leafwing-input-manager) to handle all the inputs. But I needed a way to somehow "map" animations to actions. Or at least associate them somehow. Animations are stored in handles and so I ended up (after a lot of trial and error) using a trait for this.

It looks a bit like this: 
```rust
pub trait ActionAnimationMapping: Actionlike + Copy {
    ///Get the animation data handle for this action
    fn get_animation_data_handle(
        &self,
        action_state: &ActionState<Self>,
        animation_maps: &AnimationActionMap,
    ) -> Option<Handle<AnimationData>>;

    /// Get the animation data for the animation which this one should transition into
    fn get_animation_transition_data_handle(
        &self,
        action_state: &ActionState<Self>,
        animation_maps: &AnimationActionMap,
    ) -> Option<Handle<AnimationData>>;
}
```

This allows me to very easily observe whatever `ActionState` an entity is in and grab a given animation when I need to. I would probably try and find a different way to do this now that I'm more comfortable with ECS in general that wouldn't require so much fetching of things out of resources. 

Overall - bevy is the best game engine I've used thus far (yes I have used unity, godot, etc) just because of the potential it has and the current ergonomics it provides. Being able to write a game 100% in rust is very powerful. 

As for the future of this little demo... I'm not sure if I'm too keen on starting a entirely new project and might end up waiting until Bevy has stabilized a bit with how it handles assets. However, I don't really see myself using any other game engine for (personal) projects. I might continue to hack away at this project and get some much needed polish/features on it. Who knows!


