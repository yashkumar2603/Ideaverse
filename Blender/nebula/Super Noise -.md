it is basically just 2 noises chained together in such a fashion - 
![[Pasted image 20240730234718.png]]
What happens is that, this creates the very pleasing wispy strands of gas surrounding the main blobs and that really sells the effect - 
Steps - 
1. make the arrangement as in the image, then make the value of white in the first color ramp(left) to be the scale of the second - 5
2. Syn the settings in the 2 noises, the scale, the detail at 8 and the other settings same for both.
3. Now, take the white one to the left, around 0.6ish, then take black to the right until u start seeing wispy strands around the edges of major blobs. 
4. Now, make the ratio in the first color ramp, like the white value at 10(2 times of what it was) and the vector scale at 0.25(or 1/2 of what it was), u can go for 3:1 and stuff.

### Performance - 
In the render settings, keep the max steps at 128, above it, no extra detail is formed.
Keep the step rate at lower numbers for more details, higher is for softer looks, 0.1 looks pretty fine mostly, below 0.05 there's no point, only render times increase, but no actual detail.


**Additional details (pro tip) -** 
1. take the right noise texture to 4d and then but the W to something high like 10 or 12, that will make it thin and almost noodle like. But with lot of structure and small scale detail. - 8 looks perfect, almost like nature.
2. In the second color ramp(right) add another stop, with black value changed to 0.002 and we should now have a vey tiny bit of haziness and foggyness in the cloud, basically no empty spaces, but light haze,  in which god rays might also form.
3. Tweak the anisotropy(0.3 to 0.6 works the best) after the framing is done and simultaneously tweak the light intensity.
4. 