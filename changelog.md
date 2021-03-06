# Quake II RTX Change Log

## N&C 0.1

**New Features:**

  * Anim Allfast changed to 30hz
  * Enabled QW_MOD movement mod (QW movement)
  * Enabled NODRAW surfaces (allows for invisible light sources)
  * Added support for locking sprites to rotate on z-axis only by using prefix "vrty" (without quotes) in the sprite filename.
  * Tweaked flare point light to closer resemble a flame light and changed it's "activation" model to effects/firelight/tris.md2
    
  **Fixed Issues:**

  * Fixed animated texture playback on BMODELS

## 1.3.0

**New Features:**

  * Added support for video cutscenes.
  * Added support for Depth of Field in the reference path tracing mode.
  * Added free camera controls when the game is paused. See the [Readme](readme.md) for more information.
  * Added support for selecting which display should be used for the fullscreen mode.
  * Added support for loading map-specific files with sky clusters, which should be useful for custom maps.
  * Added display of the selected inventory item name in the status bar.

**Fixed Issues:**

  * Fixed a crash that happened at map load time when a custom map has no analytic lights.
  * Reduced the noise in the `biggun` map next to the barred windows.
  * Reduced the noise from yellow lamps next to the entrance of the `jail4` map at night.

**Misc Improvements:**

  * Improved the menu settings to show units for various sliders, such as degrees or percentage.
  * Made the volume controls logarithmic instead of linear.

## 1.2.1

**Fixed Issues:**

  * Fixed the bug with broken OpenAL sound on certain maps: https://github.com/NVIDIA/Q2RTX/issues/47
  * Fixed the material on the pipe at the end of the `strike` map.
  * Fixed a typo in the `pt_enable_sprites` cvar name.
  * Fixed the handling of swap chain image layouts to avoid the black screen bug on a future driver version [200570279]
  * Restored the "projection" setting in the Video menu.

## 1.2.0

**New Features:**

  * Added support for dynamic resolution scaling that adjusts rendering resolution to meet an FPS target.
  * Added support for multiple reflection or refraction bounces.
  * Added light coloring by tinted glass.
  * Added support for security camera views on some monitors in the game.
  * Added god rays in reflections and refractions, improved god rays filtering.
  * Added a spatial denoiser for the specular channel.
  * Added support for loading custom sky (portal) light meshes from .obj files, and added portal light definitions for many maps in the base game.
  * Added triangular lights for laser beams: https://github.com/NVIDIA/Q2RTX/issues/43 
  * Added “shader balls” to the shipping builds.
  * Added cvar `pt_accumulation_rendering_framenum` to control how many frames to accumulate in the reference mode.
  * Added cvar `pt_show_sky` to make analyzing skybox geometry easier.

**Fixed Issues:**

  * Fixed the stutter caused by Steam overlay by updating to the latest version of SDL2.
  * Fixed Stroggos atmospheric scattering (sky color) and overall sky brightness.
  * Fixed light scattering on the clouds.
  * Fixed the issue with overexposed player setup screen: https://github.com/NVIDIA/Q2RTX/issues/18
  * Fixed the sudden darkening of the screen: https://github.com/NVIDIA/Q2RTX/issues/26
  * Fixed the "PF_centerprintf to a non-client" warnings that happened on the "command" map when the computers are blown up, instead of informative on-screen messages.
  * Fixed missing GI on reflections of the first person model.

**Denoising and image stability improvements:**

  * Improved image quality and temporal stability of reflections and refractions by computing correct motion vectors for reflected surfaces and surfaces visible through flat glass.
  * Disabled the pixel jitter when temporal AA is turned off.
  * Added sample jitter to the spatial denoiser to improve the noise patterns that appear after light invalidation.
  * Improved image stability around blinking lights by using the light intensities from the previous frame on gradient pixels.
  * Improved stability of indirect lighting from sphere lights by limiting their contribution.
  * Added storage scaling for all lighting channels to avoid color quantization.
  * Fixed flickering that happened when the number of dynamic lights changes.
  * Improved sharpness of textured glass and similar transmissive effects by passing them around the denoiser.
  * Added multiple importance sampling of specular reflections of direct lights.
  * Replaced sphere lights that were used for wall lamps (mostly in the “train” map) with polygon lights to reduce noise.
  * Added an upper limit on sky luminance to avoid oversampling the sky in shadowed areas and thus reduce noise from direct lights.
  * Added light sampling correction based on statistical per-cluster light visibility. The idea is, if we see that a light is usually not visible, let's not sample it so much.

**Material improvements:**

  * Metals are now rendered much better thanks to the denoiser and BRDF improvements.
  * Over 400 textures have been adjusted or repainted.
  * Removed the nonlinear transform from emissive textures, and reduced the negative bias applied to them.
  * Force light materials to be opaque to make the laser lights in mine maps appear in reflections.
  * Restore specular on materials with roughness = 1, but make specular on rough dielectrics much dimmer.
  
**Shading and BRDF improvements:**

  * Fixed scaling of diffuse and specular reflections: https://github.com/NVIDIA/Q2RTX/issues/37
  * Fixed relative brightness and spotlight terms for different light types.
  * Hemisphere sampling for indirect diffuse tuned to make the results better match the cosine-weighted sampling in reference mode.
  * Trace more specular rays for shiny metals that do not need indirect diffuse lighting.
  * Fixed the misdirection of second bounce lighting into the diffuse channel when the light path starts as specular on the primary surface: https://github.com/NVIDIA/Q2RTX/issues/25
  * Replaced regular NDF sampling for specular bounce rays with visible NDF sampling: https://github.com/NVIDIA/Q2RTX/issues/40 
  * Tuned fake specular to be closer to reference specular.
  * Added normalization of normal map vectors on load to avoid false roughness adjustments by the Toksvig factor.
  * Improved roughness correction to not happen on texture magnification, and to better handle cases like zero roughness.
  * Fixed the computation of N.V to avoid potential NaNs: https://github.com/NVIDIA/Q2RTX/issues/23

**Misc Improvements:**

  * Removed the multiplayer specific sun position setting, and changed the remaining setting to be morning by default.
  * Changed the default value of texture LOD bias to 0 for extra sharpness.
  * Use nearest filter for upscaling from 50% resolution scale (a.k.a integer scaling).
  * Made the brightness of sprites, beams, particles, smoke, and effects like quad damage be similar to overall scene brightness, to avoid having them washed out under direct sunlight.
  * Made the explosions and other sprites appear in low-roughness specular reflections.
  * Changed the flare lights to be derived from the flare model entity instead of the effect event, which makes the lights follow the flares smoothly in flight, and reduces flicker that was due to frequent light creation and destruction.
  * Removed insane and dead soldiers with `nomonsters 1`.
  * Random number generator now produces enough different sequences to cover thousands of frames for reference mode accumulation.
  * The `pt_direct_polygon_lights` cvar has a new meaning when set to -1: all polygon lights are sampled through the indirect lighting shader, for comparison purposes.
  * Moved the first person player model a bit backwards to avoid having it block reflections.

## 1.1.0

**New Features:**

  * Added music playback support - see the [Readme](readme.md) for instructions.

**Fixed Issues:**

  * Fixed the crash with message "recursive error: bad tail" that sometimes happened at the end of the `biggun` map.
  * Fixed the issue with players spawning at the wrong level entrance after loading an autosave: https://github.com/NVIDIA/Q2RTX/issues/13
  * Fixed the Linux install script to work with spaces in paths: https://github.com/NVIDIA/Q2RTX/pull/1
  * Fixed the interpretation of `pt_fake_roughness_threshold`: https://github.com/NVIDIA/Q2RTX/issues/9
  * Fixed the bright noise that appeared at the end of the `hangar2` map after closing the hangar doors.
  * Fixed the image blurring on FOV changes.
  * Added limits for sky brightness to avoid denoiser artifacts when the sky is too bright.

**Other Improvements:**

  * Re-arranged some menu options to make the menu less confusing.
  * Added the player models from the Quake II shareware demo to the package.
  * Added a menu option to invert mouse controls.
  * Enabled cl_adjustfov by default because that works better for wide screens.
  * Tweaked the tone mapper to make really dark places brighter.

## 1.0.0

**Initial Release**
