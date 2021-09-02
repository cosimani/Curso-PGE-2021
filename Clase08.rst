.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 08 - PGE 2021
===================
(Fecha: 2 de septiembre)

Biblioteca SciPy
================
 
- Espacios vectoriales, integración, interpolación, FFT, procesamiento de señales y de imagen, ...
- `Documentación de SciPy <https://docs.scipy.org/doc/scipy/reference/>`_ 


`Operaciones entre señales (ipynb) <https://colab.research.google.com/drive/1kJNpXmg6wgolewEt1p5rARsxQn6JjOKW?usp=sharing>`_ 
=====================================

.. code-block:: python
	
	import matplotlib.pyplot as plt
	plt.style.use( 'seaborn-darkgrid' )

	import numpy as np
	from scipy import signal

	# Para reproducir audio en la notebook.
	from IPython.display import Audio, display

	# señal triangular
	sample_rate = 44100
	duracion = 1
	n = np.linspace( 0, duracion, sample_rate * duracion )
	plt.xlim( [ 0, 0.005 ] )
	triangle = signal.sawtooth( 2 * np.pi * 800 * n, 0.5)
	plt.plot( n, triangle )
	plt.show()

	n = np.linspace( 0, 1, 500, endpoint = False )
	square = signal.square( 2 * np.pi * 5 * n )
	plt.plot( n, square )
	plt.ylim( -2, 2 )
	plt.show()

	n = np.linspace( 0, 5, 44100 * 5 )
	w = signal.chirp( n, f0 = 1, f1 = 1500, t1 = 5, method = 'quadratic' )
	plt.plot( n, w )
	plt.xlim( [ 0, 2 ] )
	plt.xlabel( 't (sec)' )
	plt.show()

	Audio( w, rate = 44100 )

	# Armar una señal compleja con una función seno.
	def cosenoidal( frecuencia, duracion, sample_rate, A = 1 ) :
	    n = np.linspace( 0, duracion, sample_rate * duracion )
	    return A * np.cos( 2 * np.pi * frecuencia * n )

	def generador_de_tono( frecuencia, duracion, sample_rate, A = 1 ) :
	    n = np.linspace( 0, duracion, sample_rate * duracion )
	    return A * np.sin( 2 * np.pi * frecuencia * n )

	cos_1 = cosenoidal( 300, 5, 44100 )
	cos_2 = cosenoidal( 400, 5, 44100 )
	sin_1 = generador_de_tono( 500, 5, 44100 )
	sin_2 = generador_de_tono( 200, 5, 44100 )
	suma_de_senales = cos_1 + cos_2 + sin_1 + sin_2

	plt.figure()
	t = np.linspace( 0, 5, 44100 * 5 )
	plt.plot( t, suma_de_senales )
	plt.xlim( [ 0, 0.05 ] )
	plt.grid()
	plt.show()

	Audio( suma_de_senales, rate = 44100 )

	mean = 0
	std = 1 
	sample_rate = 44100
	duracion = 5
	num_samples = seconds * sample_rate
	noise = np.random.normal( mean, std, size = num_samples)
	x = np.linspace( 0, duracion, num_samples )
	plt.plot( x, noise )
	plt.show()

	Audio( 0.5 * noise, rate = framerate )

	sin_noise = sin_1 + noise
	Audio( sin_noise, rate = sample_rate )


`Dominio de la frecuencia (ipynb) <https://colab.research.google.com/drive/1istXR5nq-D1_F7AOkzFXFXsHSdmfi84X?usp=sharing>`_ 
=====================================

.. code-block:: python
	
	# [Generador de tonos online](https://www.szynalski.com/tone-generator/)
	# [Piano virtual](https://www.musicca.com/es/piano)

	import numpy as np
	import matplotlib.pyplot as plt
	plt.style.use( 'seaborn-darkgrid' )
	from scipy import signal
	from scipy.fft import fft, fftshift
	from scipy.io import wavfile

	# Commented out IPython magic to ensure Python compatibility.
	# # Con %%capture evitamos que se publique texto en la consola, total ya sabemos que se va a
	# # publicar texto sobre la instalación de, por ejemplo, ffmpeg-python
	# 
	# %%capture
	# 
	# # Es una buena opción identificar cuál es la versión que estamos instalando de cualquiera de
	# # las herramientas, por ejemplo, ffmpeg. Podemos utilizar !pip freeze para averiguarlo para luego
	# # instalar con pip la versión específica. Para evitar que futuras versiones nos modifiquen el
	# # funcionamiento de lo que estamos programando hoy.
	# !pip install ffmpeg-python==0.2.0
	#

	t = np.linspace( 0, 5, 44100 * 5 )
	w = signal.chirp( t, f0 = 1, f1 = 1500, t1 = 5, method = 'quadratic' )
	plt.plot( t, w )
	plt.xlim( [ 0, 2 ] )
	plt.xlabel( 't (sec)' )
	plt.show()

	def cosenoidal( frecuencia, duracion = 1, sample_rate = 44100, A = 1 ) :
	    n = np.linspace( 0, duracion, sample_rate * duracion )
	    return A * np.cos( 2 * np.pi * frecuencia * n )

	def generador_de_tono( frecuencia, duracion = 1, sample_rate = 44100, A = 1 ) :
	    n = np.linspace( 0, duracion, sample_rate * duracion )
	    return A * np.sin( 2 * np.pi * frecuencia * n )

	def fourier_calculation( y, framerate ) :
	    '''Aplicando fast fourier transform'''
	    yf = fft( y )
	    N = len( y )
	    yf = 2.0 / N * np.abs( yf[ 0 : N // 2 ] )  # // es la división entera (https://www.youtube.com/watch?v=NRX6KvEP-u8)
	    xf = np.linspace ( 0.0, 1 / 2 * framerate, N // 2 )
	    return yf, xf    

	def plot_signal_espectro( y, fs = 44100, plot_type = 'plotly' ):
	    '''Plotea la señal en dominio del tiempo y luego el espectro (FFT)'''
	    
	    # Creo el espacio para plotear, una figura vacía
	    plt.figure( figsize = ( 15, 15 ) )
	    x = np.linspace( 0, len( y ) / fs, len( y ) )
	    
	    plt.subplot( 2, 1, 1 )  # Dividido en 2 filas y 1 columna, ploteo la onda en la primer fila
	    plt.plot( x, y )
	    plt.grid()  # Grilla de fondo
	    
	    # FFT
	    yf, xf = fourier_calculation( y, fs )
	    plt.subplot( 2, 1, 2 )
	    plt.xlabel( 'Freq (Hz)' )  
	    plt.ylabel( '|Y(freq)|' )
	    plt.xlim( [ 0, 2000 ] )
	    plt.plot( xf, yf )

	yf, xf = fourier_calculation( w, framerate = 44100 )
	plt.xlim( [ 0, 1000 ] )
	plt.plot( xf, yf )
	plt.show()

	cos_1 = cosenoidal( 300, 5, 44100 )
	cos_2 = cosenoidal( 400, 5 )
	sin_1 = generador_de_tono( 500, 5, 44100 )
	sin_2 = generador_de_tono( 800, 5 )
	suma_de_senales = cos_1 + cos_2 + sin_1 + sin_2

	yf, xf = fourier_calculation( suma_de_senales, framerate = 44100 )
	plt.plot( xf, yf )
	plt.xlim( [ 0, 1000 ] )

	"""
	To write this piece of code I took inspiration/code from a lot of places.
	It was late night, so I'm not sure how much I created or just copied o.O
	Here are some of the possible references:
	https://blog.addpipe.com/recording-audio-in-the-browser-using-pure-html5-and-minimal-javascript/
	https://stackoverflow.com/a/18650249
	https://hacks.mozilla.org/2014/06/easy-audio-capture-with-the-mediarecorder-api/
	https://air.ghost.io/recording-to-an-audio-file-using-html5-and-js/
	https://stackoverflow.com/a/49019356
	"""
	from IPython.display import HTML, Audio
	from google.colab.output import eval_js
	from base64 import b64decode
	import numpy as np
	from scipy.io.wavfile import read as wav_read
	import io
	import ffmpeg

	AUDIO_HTML = """
	<script>
	var my_div = document.createElement("DIV");
	var my_p = document.createElement("P");
	var my_btn = document.createElement("BUTTON");
	var t = document.createTextNode("Press to start recording");

	my_btn.appendChild(t);
	//my_p.appendChild(my_btn);
	my_div.appendChild(my_btn);
	document.body.appendChild(my_div);

	var base64data = 0;
	var reader;
	var recorder, gumStream;
	var recordButton = my_btn;

	var handleSuccess = function(stream) {
	  gumStream = stream;
	  var options = {
	    //bitsPerSecond: 8000, //chrome seems to ignore, always 48k
	    mimeType : 'audio/webm;codecs=opus'
	    //mimeType : 'audio/webm;codecs=pcm'
	  };            
	  //recorder = new MediaRecorder(stream, options);
	  recorder = new MediaRecorder(stream);
	  recorder.ondataavailable = function(e) {            
	    var url = URL.createObjectURL(e.data);
	    var preview = document.createElement('audio');
	    preview.controls = true;
	    preview.src = url;
	    document.body.appendChild(preview);

	    reader = new FileReader();
	    reader.readAsDataURL(e.data); 
	    reader.onloadend = function() {
	      base64data = reader.result;
	      //console.log("Inside FileReader:" + base64data);
	    }
	  };
	  recorder.start();
	  };

	recordButton.innerText = "Grabando con el micrófono... pulsar para finalizar";

	navigator.mediaDevices.getUserMedia({audio: true}).then(handleSuccess);


	function toggleRecording() {
	  if (recorder && recorder.state == "recording") {
	      recorder.stop();
	      gumStream.getAudioTracks()[0].stop();
	      recordButton.innerText = "Guardando la grabación... ¡espere!"
	  }
	}

	// https://stackoverflow.com/a/951057
	function sleep(ms) {
	  return new Promise(resolve => setTimeout(resolve, ms));
	}

	var data = new Promise(resolve=>{
	//recordButton.addEventListener("click", toggleRecording);
	recordButton.onclick = ()=>{
	toggleRecording()

	sleep(2000).then(() => {
	  // wait 2000ms for the data to be available...
	  // ideally this should use something like await...
	  //console.log("Inside data:" + base64data)
	  resolve(base64data.toString())
	  recordButton.innerText = "Listo"

	});

	}
	});
	      
	</script>
	"""

	def get_audio() :
	  display( HTML( AUDIO_HTML ) )
	  data = eval_js( "data" )
	  binary = b64decode( data.split(',')[1])
	  
	  process = (ffmpeg
	    .input('pipe:0')
	    .output('pipe:1', format='wav')
	    .run_async(pipe_stdin=True, pipe_stdout=True, pipe_stderr=True, quiet=True, overwrite_output=True)
	  )
	  output, err = process.communicate(input=binary)
	  
	  riff_chunk_size = len(output) - 8
	  # Break up the chunk size into four bytes, held in b.
	  q = riff_chunk_size
	  b = []
	  for i in range(4):
	      q, r = divmod(q, 256)
	      b.append(r)

	  # Replace bytes 4:8 in proc.stdout with the actual size of the RIFF chunk.
	  riff = output[:4] + bytes(b) + output[8:]

	  sr, audio = wav_read(io.BytesIO(riff))

	  return audio, sr

	grabacion, fs = get_audio()

	print( f"Cantidad de canales = { len( grabacion.shape ) }")

	length = grabacion.shape[ 0 ] / fs
	print( f"Duración = { length } segundos" )

	plot_signal_espectro( grabacion, fs )



Entregable Clase 08
===================

- Punto de partida: Crear un cuaderno Colab llamado entregable08.ipynb copiando el contenido visto e esta clase.
- Utilizando la notebook de Colab grabe con el micrófono un tono de 440 Hz generado con el generador online y también grabe la correspondiente nota con el teclado virtual.
- Modifique el código para visualizar las dos grabaciones.
- Compare los espectros de ambas grabaciones y realice algunos comentarios de lo que observa.
- Se recomienda trabajar con los demás estudiantes para llegar a la resolución más rápidamente.
- Entrar al siguiente `link para ver el registro de los entregables <https://docs.google.com/spreadsheets/d/1xbj6brqzdn3R9sfjDEP0LEjg6CwMNMOb8dBEYGmxhTw/edit?usp=sharing>`_ 
- El link de Youtube se comparte con el docente por mensaje privado de Teams.

