# Virtual Machine 1 #

## Overview ##

300 points

Category: [ReverseEngineering](../)

Tags : `#analog`

## Description ##
> The enemy has upgraded their mechanical analog computer. Start an instance to begin.
We grabbed this design doc from enemy servers: Download. We know that the rotation of the red axle is input and the rotation of the blue axle is output. Reverse engineer the mechanism and get past their checker program: (You can get a netcat server by starting an instance.)

### Solution ###

First, let's launch the file: 
[VM1.zip](https://github.com/AdithyaU89/PicoCTF23-Writeups/files/11117875/VM1.zip)
 
After extracting the file and opening it in Blender, we see this:

![image](https://user-images.githubusercontent.com/114878686/229015449-f41b3a2c-dbec-4e33-9c5f-539e7413ee51.png)

Now, change the camera angle to get a good view of the layout:

![image](https://user-images.githubusercontent.com/114878686/229015673-f11dec83-5b94-43e8-a81d-f6c7d36516af.png)

As we can see, most of these gears are simple 8, 12, 16, 24, or 40 toothed. However, there are some specific parts in the picture that seem to take inputs from both sides of gears:

![image](https://user-images.githubusercontent.com/114878686/229017037-551104b7-7964-4840-a77f-5ac12d067bc6.png)

After doing some quick research online, I found that these are called differential gears and that they take the two gear ratio inputs and average them, then send that gear ratio forward. 
Additionally, it is important to know how gear ratios work: if you spin an 8-toothed gear three times, a 24-toothed gear connected to said gear would spin once due to having 3 times as many teeth. If you are confused by how this works, do some OSINT on gear ratios before continuing.

Now, compute the ratios going on either side before each differential. Assume that the initial axle is spun once. To save space and time, I will not be doing these calculations, but I will show the calculations for each differential:

Differential 1: Entering ratios are 6 revolutions and 8 revolutions, and taking the average of these, we get: $\frac{6+8}{2}=7$ revolutions.

Differential 2: Entering ratios are 126 revolutions and 140 revolutions, and taking the average of these, we get: $\frac{126+140}{2}=133$ revolutions.

Differential 3: Entering ratios are 1330 revolutions and 1344 revolutions, and taking the average of these, we get: $\frac{1330+1344}{2}=1337$ revolutions.

Differential 4: Entering ratios are 8022 revolutions and 10696 revolutions, and taking the average of these, we get: $\frac{8022+10696}{2}=\boxed{9359}$ revolutions.

We have now recieved our final ratio: when the initial axle is spun once, the final axle spins 9,359 times. That's a high ratio! Now, for the flag, we multiply the input by 9,359:

Connect to the netcat server, multiply and you get:

![image](https://user-images.githubusercontent.com/114878686/229019885-49b34685-8092-45c9-939a-81394e41fbee.png)

Flag: `picoCTF{m0r3_g34r5_3g4d_c851b08b}`
