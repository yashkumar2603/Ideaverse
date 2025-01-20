![[Pasted image 20240730171912.png]]
This is the scenario, the main notable things are -
- The volume scatter color scatters that color the most, and the others are scattered less, that's why we can see the compliment at the bottom part of the cube more.
- The absorption color just tints the volume into the selected color, it does not apply any actual absorption calculation with the color in mind.
##  Principled volume -

![[Pasted image 20240730172407.png]]
Here, there is no absorption and scattering density, use the absorption color and main color to change them -
- change color - scattering color changes, color value changes the amount of scattering, black is 0 scattering and white is full scattering.
- Change the Absorption color - Changes the color of absorption, pure white is zero absorption, pure black is 100% absorption.

The most important thing to make 3d nebulae is the absorption color, lower the scatter and increase the density to make it very prominent and give the effect of nebula, emission is also used (which make the gas glowing basically).


With Higher Anisotropy values, the light seems to be condensed around the light source, and less spreading out.
[[Super Noise -]]