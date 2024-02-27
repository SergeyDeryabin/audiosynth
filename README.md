audiosynth
==========

Синтезатор динамических волновых форм аудио, написан на языке Javascript.
(Dynamic waveform audio synthesizer, written in Javascript.)

Генерируйте музыкальные ноты динамически
и проиграйте их в своем браузере, используя HTML5-элемент аудио  HTML5 Audio Element. (Generate musical notes dynamically
and play them in your browser using the HTML5 Audio Element.)

Не требуются никакие статические файлы. (Помимо исходника, конечно!)
(No static files required. (Besides the source, of course!))

Демонстрационный пример
----

Чтобы увидеть в действии демонстрационный пример с синтезатором audiosynth, 
посетите сайт [**http://keithwhor.com/music/**](http://keithwhor.com/music/)
(To see a demo of audiosynth in action, visit  [**http://keithwhor.com/music/**](http://keithwhor.com/music/))

Установка и настройка
------------

Подразумевая, что файл audiosynth.js находится в вашем текущем каталоге,
импортируйте пакет синтеза, используя скрипт:
(Assuming audiosynth.js is in your current directory, import package using:)

```html
<script src="audiosynth.js"></script>
```


Использование
-----

Класс синтезатор аудио  audiosynth реализует singleton-класс, '''AudioSynth'''. По умолчанию, глобальная(окно window) переменная '''Synth'''
содержит экземпляр класса синтезатор аудио '''AudioSynth'''.
(audiosynth implements a singleton class, ```AudioSynth```. By default, the global (window) variable ```Synth```
is the instance of the class.)

Любая попытка инстанцировать новый объект класса синтезатор аудио '''AudioSynth''' только создаст ссылки на
исходный объект класса синтезатор аудио '''AudioSynth''. 
(Any attempt to instantiate new ```AudioSynth``` object will only create references to
the original object.)

```javascript
Synth instanceof AudioSynth; // верно true

var testInstance = new AudioSynth;
testInstance instanceof AudioSynth; // верно true

testInstance === Synth; //  верно true
```

=== Использование класса синтезатор аудио '''AudioSynth''', для генерации .WAV-файлов...
(To use ```AudioSynth``` to generate .WAV files...)

```javascript
Synth.generate(sound, note, octave, duration);
/*
	Генерирует base64-закодированный dataURI wavefile (.WAV)-файл,содержащий ваши данные.
	(Will generate a base64-encoded dataURI wavefile (.WAV) containing your data.)

	sound
		числовой индекс или строка, относящаяся к звуковому профилю (по идентификатору или по имени, соответственно)
		(a numeric index or string referring to a sound profile (by id or name, respectively))
	
	note
		нота, которую хотите проиграть (A, B, C, D, E, F, G). Поддерживаются диезы (т.е. C#), но не бемоли.
		(Используйте соответствующий диез!)
		(the note you wish to play (A,B,C,D,E,F,G). Supports sharps (i.e. C#) but not flats.
		(Use the respective sharp!))
	
	octave
		номер октавы у нот, которые хотите проиграть
		(the octave # of the note you wish to play)
		
	duration
		длительность(в секундах) ноты
		(the duration (in seconds) of the note)
*/
```

=== Вы можете немедленно проиграть ноты, используя...
(You can play notes instantly using...)

```javascript
/*
	Те же параметры как и в функции генерации файла Synth.generate,
	только эта функция создает HTML-элемент аудио HTML Audio element, играет его и вызгружает его
 	после завершения. (Same arguments as Synth.generate,
	only this creates an HTML Audio element, plays it, and unloads it upon completion.)
*/
Synth.play(sound, note, octave, duration);
```

Вы можете также создать специфичные инструменты(объекты, что ссылаются на функцию генерации файла .generate и на функцию немедленного проигрывания ноты), связанные со специфичными звуками).
(You may also create individual instruments(objects that reference .generate and .play, bound to specific
sounds).)

```javascript
var piano = Synth.createInstrument('piano'); // профиль звука 'фортепьяно'

// немедленно проигрывает ноту C4 в течение 2 с, используя профиль звука 'фортепьяно' 
piano.play('C', 4, 2); // plays C4 for 2s using the 'piano' sound profile 
```


Профили звука
--------------

'''AudioSynth''' идет с четырьмя профилями звука по умолчанию.
(```AudioSynth``` comes with four default sound profiles.)

__piano__ (id 0)  // профиль звука 'фортепьяно'

__organ__ (id 1)  // профиль звука 'орган'

__acoustic__ (id 2) // профиль звука 'аккустическая гитара'

__edm__ (id 3) // профиль звука 'Brutal death metal'

```javascript
  						   // проигрывайте на своей акустической гитаре!
var acoustic = Synth.createInstrument('acoustic'); // play with your acoustic guitar!						 
```


Изменение настроек
-----------------

Низкая производительность? Частота дискретизации по умолчанию для синтезатора AudioSynth составляет 44100 Гц (качество CD). Это может быть затратным бременем на вашем браузере.(Poor performance? The default sampling rate for AudioSynth is 44100Hz (CD quality). This can be taxing on your browser.)

Чтобы изменить частоту дискретизации, используйте функцию установки частоты дискретизации '''Synth.setSampleRate(n)'''
Обратите внимание на то, что более низкие частоты дискретизации приравняются к более плохому качеству звука, специально для более высоких нот.(To change the sampling rate, use ```Synth.setSampleRate(n)```
Please note that lower sampling rates will equate to poorer sound quality, especially for higher notes.)

```javascript
// Можно установить значения только между 4000 Гц и 44100 Гц.
// Can only set values between 4000Hz and 44100Hz.
			    // устанавливаем частоту дискретизации в 20000Hz
Synth.setSampleRate(20000); // sets sample rate to 20000Hz
		       // возвращает частоту дискретизации  20000
Synth.getSampleRate(); // returns 20000
```
Объем слишком? Измельченный объем выборки так же.
Volume a bit much? Adust the volume of the sample similarly.

```javascript
		       // устанавливаем громкость в 100%
Synth.setVolume(1.00); // set volume to 100%

		       // нет, ожидайте, 40%.
Synth.setVolume(0.40); // no, wait, 40%.

			 // еще лучше.
Synth.setVolume(0.1337); // even better.
		   // возвращает  0.1337
Synth.getVolume(); // returns 0.1337
```


Расширенное использование
--------------
Дополнительные профили звука могут быть загружены, используя функцию загрузки профиля звука '''Synth.loadSoundProfile ()'''(Additional sound profiles can be loaded using ```Synth.loadSoundProfile()```)

```javascript
// Загрузите профиль звука из объекта...
// Load a sound profile from an object...
Synth.loadSoundProfile({
	// name it
	name: 'my_sound',
	// WIP: возвратите отрезок времени в секундах, атака длится
	// WIP: return the length of time, in seconds, the attack lasts
	attack: function(sampleRate, frequency, volume) { ... },
	// WIP: возвратите число, представляющее уровень затухания сигнала.
	// WIP: return a number representing the rate of signal decay.
	// larger = faster decay
	dampen: function(sampleRate, frequency, volume) { ... },
	// волновая функция: вычислите амплитуду своей синусоидальной волны на основе i (индекс)
	// wave function: calculate the amplitude of your sine wave based on i (index)
	wave: function(i, sampleRate, frequency, volume) {
		/*
		Здесь у нас есть доступ  к...
		this.modulate: массив загруженных частот
		this.vars: любые временные переменные, которые вы хотите отслеживать
		(Here we have access to...
		this.modulate : an array of loaded frequency
		this.vars : any temporary variables you wish to keep track of)
		*/
	}
	
});
```

Грубое руководство по генерации формы волны может быть найдено в http://keithwhor.com/music/(A rough guide to waveform generation can be found at http://keithwhor.com/music/)


Отладка
---------

Если вы зависаете на генерации нот(для профилей по умолчанию или нестандартных звуковых профилей), то используйте '''Synth.debug ()''', чтобы включить режим отладки. (If you're hanging on note generation (for default or custom sound profiles), use ```Synth.debug()``` to enable debugging.
Это журналируем  времена генерации нот в вашей консоли.(This will log note generation times in your console.)


Кредиты и подтверждения
----------------------------

Особая благодарность Альберту Паму(Albert Pham) (http://www.sk89q.com/) за динамическую генерацию .WAV -файла, облегчение работы с помощью (http://www.sk89q.com/playground/jswav/) и благодарность Хэсен эль Джуди(Hasen el Judy) (http://dev.hasenj.org/post/4517734448) за информацию о синтезе Karplus-Strong
String Synthesis. (Special thanks to Albert Pham (http://www.sk89q.com/) for Dynamic .WAV file generation,
the work off of which this is based (http://www.sk89q.com/playground/jswav/)
and Hasen el Judy (http://dev.hasenj.org/post/4517734448) for information regarding Karplus-Strong
String Synthesis.)


Дополнительные материалы для чтения
---------------

__.WAV Audio Files__

[**http://en.wikipedia.org/wiki/.WAV_file**](http://en.wikipedia.org/wiki/.WAV_file)


__Sound Synthesis__
[**http://www.acoustics.salford.ac.uk/acoustics_info/sound_synthesis/**](http://www.acoustics.salford.ac.uk/acoustics_info/sound_synthesis/)

 

__"acoustic" sound profile__ generated using __Karplus-Strong String Synthesis__:
[**http://en.wikipedia.org/wiki/Karplus%E2%80%93Strong_string_synthesis**](http://en.wikipedia.org/wiki/Karplus%E2%80%93Strong_string_synthesis)
[**http://music.columbia.edu/cmc/musicandcomputers/chapter4/04_09.php**](http://music.columbia.edu/cmc/musicandcomputers/chapter4/04_09.php)


Контакты
-------

Не стесняйтесь посылать мне по электронной почте в keithwhor в com точки Gmail или следуйте за мной на Twitter, @keithwhor. Если Вам нравится, не стесняйтесь совместно использовать!:) Всегда ценивший.
(Feel free to e-mail me at keithwhor at gmail dot com or follow me on Twitter, @keithwhor. If you like, feel free to share! :) Always appreciated.)
