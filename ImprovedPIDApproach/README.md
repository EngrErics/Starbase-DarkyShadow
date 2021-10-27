`ImprovedPIDApproach` is a PID Controller that has been modified to approximate a Motion Profiled PID Controller for the Starbase game problem of approaching asteroids.

`Installation:`

Make sure to set up your Range Finder&#39;s Fields

`RangeFinderOnState` can be renamed to `Ap`

`RangeFinderSearchLength` can be maxed at `1000`, note that Approach will not begin until you within the m range

`RangeFinderDistance` should be renamed to `M`

This script assumes a transponder has been installed so that you can have 2 different tunings on line 3 and 4 for whether you are inside the safezone or out of the safezone. If there is no Transponder present on the ship then you should remove the `:insideSafeZone` on line 2 with `0`, alternatively you can just delete the entire `+:insideSafeZone`;.

**Tuning Parameters:** Only Change These Values on lines 1, 3, and 4

I have placed these in order of importance for you to get your desired results

`s=17` Setpoint, what distance from asteroid you want to approach to

`m=500` Required minimum distance to asteroid that approach will work/begin

`Kp=0.06` Coefficient of Proportional term, increase for a faster more aggressive approaches

`Kd=5.8` Coefficient of Derivative term, need to increase this when increasing Kp. Remember that Kd is the opposition to Kp, like my sled being pulled from both sides to keep it controlled while moving forward analogy. If you don&#39;t fly forward when initially testing, you need to reduce this. If you overshoot your target, you need to increase this.

`H=0.5` This value should always be between 0 and 1. Lower values reduce oscillations in the d term at the cost of losing responsiveness. Higher values cause the d term to be more responsive. For more aggressive tunings you will have to use a higher value here. If you see your rev thrusters pulsing too much during flight and especially if it causes you to veer off throughout the middle of the approach, decreasing this will help at the cost of not being able to user larger Kp values. This value affects a lowpass filter that has been applied to the D term on line 5. If unsure keep between 0.5 and 0.8.

`t=20` Time spent to limit/rampup acceleration, in multiples of 0.4s, i.e. 20=8sec reducing this number will cause a faster initial rampup. If you veer off course during the initial start increasing this number will more gracefully start the approach. Think of this as a softstart.

`ib=5` Distance within (meters), where Integration will begin to be performed. Another way to decrease the amount of time that you spend centering on the setpoint at the very end is to start the integration a little sooner, you could try changing this to 6,7,10 etc…

`Ki=0.001` Coefficient of Integral term. This term is what performs the end centering on the setpoint. If you feel that at the end it takes too long to hit the setpoint, you can try increasing this to 0.002 or even further

