## `ImprovedPIDApproach` is a PID Controller that has been modified to approximate a Motion Profiled PID Controller for the Starbase game problem of approaching asteroids.

NOTE: V2 is vastly better than the original but requires fcuforward and fcubackward to be renamed to FWD and BWD respectively. What makes V2 better than the original is that in V2 the Proportional Term is capped by a value between 0 to 1000. This "Tricks" the PID algorithm into applying a constant thrust throughout the middle portion of our approach. In the code, this tunable variable is n and can be found on lines 3 and 4. Lower values of n will result in lower speeds throughout the middle portion of the approach and larger values will result in higher speeds. My recommendation is to set n=500 and then tune Kp and Kd to have a graceful approach to the setpoint. Other changes that have ocurred is that now the h filter value is f and has been inverted, meaning that if your h=0.8 in V1 then in V2 f=0.2. The great thing about V2 is that if you tune and find good values of Kp and Kd for a tuning in the safe-zone (more aggressive) you can easily keep it the same and simply modify n lower for outside the safe-zone (more conservative).

Tuning of this System requires some understanding. I made a manual that teaches PID in a very easy way and explains how it's been modified for us.

Intro Video https://youtu.be/rJoOOk1pjak

You can see a good example of this algorithm in action here @ 13min 40sec https://www.youtube.com/watch?v=voRbRqMvug4


### `!!! DOWNLOAD THE PDF MANUAL !!!`
Credit is given in the manual to a different version by player: Whitestrake and one snipet of code was used (d\*d>g+e\*e>1) from his version. Thanks for your work and the opensource version you provided. I hope my version is paying it forward.

<br>
ImprovedPIDapproach.yolol is always more up to date than the version at the end of the manual

This Readme is always more up to date than the manual
<br>

Donations are greatly welcomed and appreciated but not expected. If you like the work or if you are making money selling ships with the work, you may donate to Darkyshadow in game via mail or to https://paypal.me/engineereric for Paypal.

<br>

### `Installation:`

Make sure to set up your Range Finder&#39;s Fields

`RangeFinderOnState` can be renamed to `Ap`

`RangeFinderSearchLength` can be maxed at `1000`, note that Approach will not begin until you within the m range

`RangeFinderDistance` should be renamed to `M`

A Hybrid Button or Simple Button needs to be added

`ButtonState` can be renamed to `Ap`

`ButtonOnStateValue` set to `1`, `ButtonOffStateValue` set to `0`, this is only applicable to a Hybrid Button

`ButtonStyle` should be `1`, we want a basic toggle functionality

Note: This script assumes a transponder has been installed so that you can have 2 different tunings on line 3 and 4 for whether you are inside the safezone or out of the safezone. If there is no Transponder present on the ship then you should repalce the `:insideSafeZone` on line 2 with `0`, alternatively you can just delete the entire `+:insideSafeZone`.

<br>
<br>

### `Tuning Parameters:`

Only Change These Values on lines 1, 3, and 4

I have placed these in order of importance for you to get your desired results

`s=17` Setpoint, what distance from asteroid you want to approach to

`m=500` Required minimum distance to asteroid that approach will work/begin. While the approach is enabled, any time the rangefinder has a value greater than this the approach will be in a safe state not flying the ship but instead waiting to enter/reenter the approach sequence. For maximum range (i.e. doing approach from max distance, set this to 1000)  

`t=20` Time spent to limit/rampup acceleration, in multiples of 0.4s, i.e. 20=8sec, reducing this number will cause a faster initial rampup. If you veer off course during the initial start increasing this number will more gracefully start the approach. Think of this as a softstart.

`Kp=0.06` Coefficient of Proportional term, increase for a faster more aggressive approaches.

`Kd=5.8` Coefficient of Derivative term, need to increase this when increasing Kp. Remember that Kd is the opposition to Kp, like my sled being pulled from both sides to keep it controlled while moving forward analogy from the manual. If you don&#39;t fly forward when initially testing, you need to reduce this. If you overshoot your target, you need to increase this.

`H=0.5` This value should always be between 0 and 1. Lower values reduce fast changes in the d term at the cost of losing responsiveness. Higher values cause the d term to be more responsive. For more aggressive tunings you will have to use a higher value here. If you see your rev thrusters pulsing too much during flight and especially if it causes you to veer off throughout the middle of the approach, decreasing this will help at the cost of not being able to user larger Kp values. This value affects a lowpass filter that has been applied to the D term on line 5. If unsure keep between 0.5 and 0.8.

`g=0.200` This value determines the stop condition of the approach software loop. The end is determined by the velocity that the ship is moving while at the setpoint. 0.200 in the example refers to the velocity the ship can still be moving when at the setpoint when determining that the end of the approach has occurred. If you want the approach end to be recognized sooner, increase this value. 

`ib=5` Distance within (meters), where Integration will begin to be performed. Another way to decrease the amount of time that you spend centering on the setpoint at the very end is to start the integration a little sooner, you could try changing this to 6,7,10 etc…

`Ki=0.001` Coefficient of Integral term. This term is what performs the end centering on the setpoint. If you feel that at the end it takes too long to hit the setpoint, you can try increasing this to 0.002 or even further

