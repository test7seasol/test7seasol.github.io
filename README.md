# test7seasol.github.io

// Hello World

// Image to text get text from image firebaseml kit image to text

    <uses-permission android:name="android.permission.INTERNET" />

       <meta-data
                android:name="com.google.firebase.ml.vision.DEPENDENCIES"
                android:value="ocr" />
                
    
        implementation(platform("com.google.firebase:firebase-bom:33.6.0"))
        implementation("com.google.firebase:firebase-ml-vision:24.0.3")


        val bitmap = BitmapFactory.decodeResource(
            resources, R.drawable.testimg
        ) // Replace with your image
        extractTextWithFirebase(bitmap)
        }
    
        private fun extractTextWithFirebase(bitmap: Bitmap) {
            try {
                val image = FirebaseVisionImage.fromBitmap(bitmap)
                val recognizer = FirebaseVision.getInstance().onDeviceTextRecognizer
    
                recognizer.processImage(image)
                    .addOnSuccessListener { firebaseVisionText: FirebaseVisionText ->
                        // Extracted text
                        val recognizedText = firebaseVisionText.text
                        Log.d("TextRecognition", "Recognized Text: $recognizedText")
                    }.addOnFailureListener { e: Exception? ->
                        // Handle failure
                        Log.e("TextRecognition", "Text recognition failed", e)
                    }
            } catch (e: Exception) {
                Log.e("TextRecognition", "Error processing image", e)
            }
        }



// Google STT TTS
     
        private val SPEECH_REQUEST_CODE = 1
        
     if (ContextCompat.checkSelfPermission(
                    this, Manifest.permission.RECORD_AUDIO
                ) != PackageManager.PERMISSION_GRANTED
            ) {
                ActivityCompat.requestPermissions(this, arrayOf(Manifest.permission.RECORD_AUDIO), 100)
            }
    
            bind.apply {
                btnsttid.onClick {
                    startGoogleVoiceInput()
                }
            }

               private fun startGoogleVoiceInput() {
        val intent = Intent(RecognizerIntent.ACTION_RECOGNIZE_SPEECH).apply {
            putExtra(
                RecognizerIntent.EXTRA_LANGUAGE_MODEL, RecognizerIntent.LANGUAGE_MODEL_FREE_FORM
            )
            putExtra(RecognizerIntent.EXTRA_LANGUAGE, "en-US") // Set desired language
            putExtra(RecognizerIntent.EXTRA_PROMPT, "Speak something...")
        }

        try {
            startActivityForResult(intent, SPEECH_REQUEST_CODE)
            } catch (e: ActivityNotFoundException) {
                Toast.makeText(
                    this, "Google Voice Input not supported on your device", Toast.LENGTH_SHORT
                ).show()
            }
        }
    
        override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
            super.onActivityResult(requestCode, resultCode, data)
    
            if (requestCode == SPEECH_REQUEST_CODE && resultCode == RESULT_OK && data != null) {
                val results = data.getStringArrayListExtra(RecognizerIntent.EXTRA_RESULTS)
                val spokenText = results?.get(0) ?: ""
                ("Text: $spokenText").log()
    //            Toast.makeText(this, "You said: $spokenText", Toast.LENGTH_LONG).show()
            }
        }
