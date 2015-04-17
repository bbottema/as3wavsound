  * *release v0.9: (finally) fixed playback of sample rates below 44khz!*
  * *release v1.0: will also support 8khz sample rate*

***Introducing AS3WavSound as AWS***

The Flex SDK does not natively support playing (embedded) .wav files. Thus far developers worked around this using ugly hacks (generating swf bytedata to trick the Flash Player).

Not anymore.

AWS in the slimmest sense simply a copy of Adobe's Sound class (http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/media/Sound.html). It mimics the Sound class but has support for playing back WAVE data. You don't need this sound class if you are working with the Flash IDE or Flex Builder, as they convert .wav data directly to Sound objects. The open source SDK compiler however, does not support this feature. But it does now!

AWS currently needs Flash Player 10 or higher.

**How it works**

AWS uses a Wav decoder that converts !ByteData into mono / stereo, 44100 / 22050 / 11025 samplerate, 8 / 16 bitrate sample data, that is playable by the Sound class using the [http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/events/SampleDataEvent.html SampleDataEvent technique]. This way no obscure swf generation hack is necessary, which has become the defacto standard work-around. Sample rates below 44100hz are upsampled to 44100hz.

To add .wav files to your project you embed a .wav file as a !ByteArray with mimetype 'application/octet-stream' and AWS will be able to decode this and playback this sound.

**Example**

<pre>
public class Demo {
		
	[Embed(source = "assets/drum_loop.wav", mimeType = "application/octet-stream")]
	public const DrumLoop:Class;
		
	public function foo():void {
		var drumLoop:WavSound = new WavSound(new DrumLoop() as ByteArray);
		var playingChannel = drumLoop.play();
		// playingChannel.stop();
	}
}
</pre>

It's that easy. No generating swf's required and no Flash IDE or Flex Builder required. The library totals up to about 40kb (a swc component is planned for the future).

***Documentation***

<div>
 <div class="vt" id="wikimaincol">
 <h2><a name="About_this_documentation"></a>About this documentation<a href="#About_this_documentation" class="section_anchor"></a></h2><p>Documentation for AS3WavSound may seem minimal, but there's a good reason for this.  </p><ul><li><i>A <a href="/p/as3wavsound/wiki/WavSound">WavSound</a> object doesn't do anything particular a Sound object doesn't do as well</i> </li></ul><p>The <a href="/p/as3wavsound/wiki/WavSound">WavSound</a> class, AS3WavSound's pivot point, was designed to use the same API as the existing Sound classes and acts exactly the same. In fact, it adds no additional features <i>except the ability to playback WAVE sound data</i>. Therefor it makes sense that no more documentation is needed than Adobe already provides on the entire Sound framework! </p><p>Due to limitations of Adobe's Sound framework (specifically the <a href="http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/media/SoundChannel.html" rel="nofollow">SoundChannel</a> class is made <i>final</i>), backwards compatibility has been dropped, but the <a href="/p/as3wavsound/wiki/WavSound">WavSound</a> as well as the <a href="/p/as3wavsound/wiki/WavSoundChannel">WavSoundChannel</a> are designed to work exactly like Sound and good old SoundChannel. The intent is to keep the API the same while simply adding Wave sound support. You just can't play mp3 files with this library anymore, because we're not extending the existing sound library anymore. </p><h2><a name="Transparent_API"></a>Transparent API<a href="#Transparent_API" class="section_anchor"></a></h2><p>There is no additional API to the standard Flash Sound API. AS3WavSound simply completely mimicks the existing system. </p><p>The reason for aforementioned grade of backwards API compatibility is because AS3WavSound aims to be as simple to use as possible and simply fill the gap of WAVE playback support. Nothing more. Certainly not another library API for users to learn. </p><p><strong>Example</strong> </p><pre class="prettyprint"><span class="kwd">public</span><span class="pln"> </span><span class="kwd">class</span><span class="pln"> </span><span class="typ">Demo</span><span class="pln"> </span><span class="pun">{</span><span class="pln"><br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <br>&nbsp; &nbsp; &nbsp; &nbsp; </span><span class="pun">[</span><span class="typ">Embed</span><span class="pun">(</span><span class="pln">source </span><span class="pun">=</span><span class="pln"> </span><span class="str">"assets/drum_loop.wav"</span><span class="pun">,</span><span class="pln"> mimeType </span><span class="pun">=</span><span class="pln"> </span><span class="str">"application/octet-stream"</span><span class="pun">)]</span><span class="pln"><br>&nbsp; &nbsp; &nbsp; &nbsp; </span><span class="kwd">public</span><span class="pln"> </span><span class="kwd">const</span><span class="pln"> </span><span class="typ">DrumLoop</span><span class="pun">:</span><span class="typ">Class</span><span class="pun">;</span><span class="pln"><br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <br>&nbsp; &nbsp; &nbsp; &nbsp; </span><span class="kwd">public</span><span class="pln"> </span><span class="kwd">function</span><span class="pln"> foo</span><span class="pun">():</span><span class="kwd">void</span><span class="pln"> </span><span class="pun">{</span><span class="pln"><br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span class="kwd">var</span><span class="pln"> drumLoop</span><span class="pun">:</span><span class="typ">WavSound</span><span class="pln"> </span><span class="pun">=</span><span class="pln"> </span><span class="kwd">new</span><span class="pln"> </span><span class="typ">WavSound</span><span class="pun">(</span><span class="kwd">new</span><span class="pln"> </span><span class="typ">DrumLoop</span><span class="pun">()</span><span class="pln"> </span><span class="kwd">as</span><span class="pln"> </span><span class="typ">ByteArray</span><span class="pun">);</span><span class="pln"><br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </span><span class="kwd">var</span><span class="pln"> channel</span><span class="pun">:</span><span class="typ">WavSoundChannel</span><span class="pln"> </span><span class="pun">=</span><span class="pln"> drumLoop</span><span class="pun">.</span><span class="pln">play</span><span class="pun">();</span><span class="pln"><br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; channel</span><span class="pun">.</span><span class="pln">stop</span><span class="pun">();</span><span class="pln"><br>&nbsp; &nbsp; &nbsp; &nbsp; </span><span class="pun">}</span><span class="pln"><br></span><span class="pun">}</span></pre><h2><a name="Current_state_/_Roadmap"></a>Current state / Roadmap<a href="#Current_state_/_Roadmap" class="section_anchor"></a></h2><p>AS3WavSound is mostly finished except for the following features and remaining bugs: </p><ol><li>complete <a href="http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/media/SoundTransform.html" rel="nofollow">SoundTransform</a> support, such as leftToleft en rightToLeft properties (currently only panning and volume are supported) </li><li>streaming support (ie. Sound.load() or Sound.loadWav()) </li><li>better upsampling algorithm. The current one is a rudimentary one that provides low quality upsampling (quality degrades for lower source rates). </li></ol><p><a href="/p/as3wavsound/wiki/SoundChannelBug">Quick note on the SoundChannel problem</a> </p><h2><a name="About_performance"></a>About performance<a href="#About_performance" class="section_anchor"></a></h2><p>A performance hit is to be expected the moment one generates sound on the fly using the <a href="http://www.adobe.com/devnet/flash/articles/dynamic_sound_generation/index.html" rel="nofollow">Adobe sanctioned SAMPLE_DATA event approach</a>. </p><p>However, AS3WavSound has completely minimized this hit by pooling all playing WavSounds into a single Sound. In effect, in this fashion always exactly one Sound is really playing samples generated and mixed from all WavSounds active, combined. It is still not as fast as native playback however, in which Flash directly delegates to the sound card. </p><h2><a name="Some_factoids">
 </div>
 </div>
---
***WavSound***
---
<div>
 <div class="vt" id="wikimaincol">
 <p><a href="/p/as3wavsound/wiki/WavSound">WavSound</a> is the class in the AWS library that encapsulates the playback API just as Adobe's <a href="http://www.adobe.com/livedocs/flash/9.0/ActionScriptLangRefV3/flash/media/Sound.html" rel="nofollow">Sound</a> class does. It supports all methods the standard Sound class does. On calling <tt>.play()</tt> a <a href="/p/as3wavsound/wiki/WavSoundChannel">WavSoundChannel</a> object is returned for runtime playback operations which has the same API as Adobe's <a href="http://www.adobe.com/livedocs/flash/9.0/ActionScriptLangRefV3/flash/media/SoundChannel.html" rel="nofollow">SoundChannel</a>. </p>
 </div>
 </div>
---
***WavSoundChannel***
---
 <div>
 <div class="vt" id="wikimaincol">
 <p><a href="/p/as3wavsound/wiki/WavSoundChannel">WavSoundChannel</a> is the class in the AWS library that encapsulates the playback API just as Adobe's <a href="http://www.adobe.com/livedocs/flash/9.0/ActionScriptLangRefV3/flash/media/SoundChannel.html" rel="nofollow">SoundChannel</a> class does. It supports all methods the standard SoundChannel<a href="/p/as3wavsound/w/edit/SoundChannel">?</a> class does. </p>
 </div>
 </div>
---
***SoundChannel Bug***
---
 <div>
 <div class="vt" id="wikimaincol">
 <h2><a name="Quick_note_on_the_problem"></a>Quick note on the SoundChannel<a href="/p/as3wavsound/w/edit/SoundChannel">?</a> problem<a href="#Quick_note_on_the_problem" class="section_anchor"></a></h2><p>To maintain complete transparent backwards compatibility with the Sound API, AS3WavSound would need to support <i>soundChannel.stop()</i>, which currently is impossible. The reason for this is because SoundChannel<a href="/p/as3wavsound/w/edit/SoundChannel">?</a> has been declared <i>final</i> by Adobe's developers. And so it cannot be subclassed to redefine behavior for stop() to stop playing a <a href="/p/as3wavsound/wiki/WavSoundChannel">WavSoundChannel</a>. No work-around is currently known. </p><p>A solution could be to get support for a SOUND_STOP event alongside the SOUND_COMPLETE event. But alas. </p><h2><a name="current_state"></a>current state<a href="#current_state" class="section_anchor"></a></h2><p>Backwards compatibility has been dropped in favor of a working solution. Instead all Adobe's sound classes have a analog wav counterpart now. </p><ul><li>Sound -&gt; WavSound </li><li>SoundChannel -&gt; WavSoundChannel </li></ul><p>Now wavSoundChannel.stop() works for .wav files. </p><p></p>
 </div>
 </div>
