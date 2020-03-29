# Safety Notice

Before using your _Binho Nova Multi-Protocol USB Host Adapter,_ let's review some basic safety information to keep in mind while working with electronics in order to avoid injury or damage.

### Electrical Isolation

{% hint style="warning" %}
The _Binho Nova Multi-Protocol USB Host Adapter_ is **not** electrically isolated from the PC.
{% endhint %}

### Ground Current Safety

Caution should be taken when using the host adapter in the presence of a ground loop. Ground loops can easily occur in test bench setups when the host adapter is not the only ground path between your test circuit and the host computer. This is not necessarily bad, but requires an additional awareness.

**Common Ways a Ground Loop Can Exist:**

* Other USB devices \(such as programmers\) are connected to the test circuit, or the test circuit itself is plugged into the USB port on your computer. In addition to the host adapter's ground connection, the test circuit's ground is also connected to the PC's ground through another USB port.
* Non-isolated power supplies. Most AC power supplies with 3-prong plugs will short the MAINS earth ground pin to the power supply ground output. That includes your PC's ground. If your test circuit is powered from a 3-prong wall power supply and your PC is also plugged in, that will form another ground path. Keep in mind that if you're using a laptop that's not plugged in, even an attached external monitor or printer will create a ground loop.

**Common Ways Damage Can Occur:**

* When connecting or disconnecting wires, one of the ground signals from the host adapter is accidentally brushed against a power supply pin on the test circuit, such as +5V. If there are no other ground paths between the test circuit and the computer, nothing will happen. However, if there is a ground path, then current will flow from that voltage supply through the host adapter's ground pin, through the USB cable and the host PC, and then through the secondary ground connection—either MAIN earth ground or another USB port, back to the ground on the test circuit. Basically, that is the same as shorting out the voltage supply on your test circuit, but it uses the host adapter and your host PC as the short circuit, which could damage all components in the loop.
* What if the test circuit's ground reference isn't at the same voltage as the ground loop connection? For instance, if your circuit is powered by a bipolar power supply used to produce +10 volts and -10 volts, and then your circuit uses the -10 volt rail as its ground voltage, but there exists a ground loop through MAINS earth ground to the power supply's 0 volt output, then effectively the ground on your test circuit is actually -10 volts relative to the host PC. Connecting your _Binho Multi-Protocol USB Host Adapter_ will immediately short out the test circuit's power supply and potentially damage all devices present in the loop.

### **Identifying if a Ground Loop is Present**

To identify a potential ground loop between the host adapter and the test circuit, you could check the resistance between the test circuit ground and the Binho host adapter ground. While the host adapter is connected to the PC, if the resistance reads infinite on a multi-meter, then the grounds are isolated. Otherwise, they are connected, and a ground loop exists.

{% hint style="info" %}
If a ground loop is present, extra care should be taken, as highlighted below, before connecting the host adapter ground to the test circuit ground.
{% endhint %}

If you believe there is a ground loop between the test circuit and the host PC but you are uncertain if the grounds on both sides you plan to use are at the same potential, there is a quick test you can perform with a multi-meter. If you happen to have a large resistor \(&gt; 10K ohm\), there is an additional test you can perform.

1. Connect the host adapter to the PC but not the test circuit.
2. Measure the voltage between the ground pin on the host adapter and the ground pin on the test circuit.
3. If there is a ground loop and you measure a voltage greater than about +/- 100mV, then a common mode ground current may occur when they are connected, damaging your equipment.
4. If there is a ground loop and you measure a voltage smaller than about +/- 100mV, then it is safe to connect the ground pins.
5. If there is not a ground loop or you are not sure there is a ground loop, then the voltage may drift significantly. If you are SURE there is no ground loop, then it is safe to connect the grounds.

If you are not sure there is a ground loop or would like to perform another test anyway, connect the resistor \(~10K\) between the two grounds and then measure the voltage across the resistor.

* If you see a voltage that indicates a noticeable current, then there is a ground loop between devices and you should not connect the grounds together.
* If you see an insignificant voltage across the resistor, then either there is no ground loop or there is a ground loop, but both grounds are at the same reference. It is safe to connect the host adapter.

### **Identifying If Your Test Circuit Is Isolated From The PC**

The test circuit's local ground is isolated from the host PC when one of the following is true:

* The test circuit is battery-powered and has no other electrical connections to the host PC or devices powered from MAINS power.
* The test circuit is powered from an isolated power supply that does NOT short MAINS earth ground to the output ground. Bench top supplies with a separate green earth ground terminal do this. USB wall adapters also do this. Common AC power adapters \(chargers, "wall warts"\) with 2-prong plugs are also isolated. Most power supplies do have transformers that can provide isolation if implemented properly.
* The Host PC is a laptop running from a battery or is plugged into an instrumentation isolation transformer. Note that normal isolation transformers connect earth ground for human safety reasons.

{% hint style="warning" %}
_Warning:_ When working in an electrically isolated state, keep in mind that floating grounds can be dangerous to the operator. When operating equipment with a floating ground, please review and follow appropriate safety measures.
{% endhint %}

### **Using an Isolated Wall Adapter to Power the Test Circuit**

Using isolated wall adapters such as USB wall adapters to power the test circuit will isolate its ground from MAINS ground, although that does not always eliminate ground loops. For example, if the test circuit was connected to the same computer that the host adapter is connected to, then a ground loop is formed.

### **Is the Binho Host Adapter Safe to Use in the Presence of Ground Loops?**

Yes, it is completely safe to use the _Binho Nova Multi-Protocol USB Host Adapter_ as long as both grounds are at the same voltage level and as long as you only connect the host adapter ground to the ground of the test circuit.

