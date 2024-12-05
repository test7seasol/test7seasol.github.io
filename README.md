# test7seasol.github.io

// Hello World

// Pick a image from camera opencamera camera image pick camera pick camera image
   
    private val CAMERA_REQUEST_CODE = 100
      
        <provider
                  android:name="androidx.core.content.FileProvider"
                  android:authorities="${applicationId}.fileprovider"
                  android:enabled="true"
                  android:exported="false"
                  android:grantUriPermissions="true">
                  <meta-data
                      android:name="android.support.FILE_PROVIDER_PATHS"
                      android:resource="@xml/file_paths" />
              </provider>
              
    fun openCamera() {
        val intent = Intent(MediaStore.ACTION_IMAGE_CAPTURE)
        val photoFile = createImageFile()
        imageUri = FileProvider.getUriForFile(
            this, "${packageName}.fileprovider", photoFile
        )
        intent.putExtra(MediaStore.EXTRA_OUTPUT, imageUri)
        startActivityForResult(intent, CAMERA_REQUEST_CODE)
    }

            onActivityResult
           if (requestCode == CAMERA_REQUEST_CODE && resultCode == RESULT_OK) {
                  try {
                      // Decode the captured image to a Bitmap
                      val bitmap = MediaStore.Images.Media.getBitmap(contentResolver, imageUri)
                      ("FAT: Bitmap HGEre: " + imageUri).log()
                      (imageUri.toString()).startUcrop(this@MainMainActivity)
      //                extractTextWithFirebase(bitmap)
                  } catch (e: IOException) {
                      e.message?.log()
                  }
              }
        

// Google ad error addmob lib errror ad issues ad error

      in menifest
     <property
                android:name="android.adservices.AD_SERVICES_CONFIG"
                android:resource="@xml/ga_ad_services_config"
                tools:replace="android:resource" />


// Synonyms

      https://www.wordsapi.com/mashape/words/apple/synonyms?when=2024-12-03T05:22:20.276Z&encrypted=8cfdb18ee723919bea9207bfea58bcbbaeb22d0932f891b8

      // Dictionary API

      https://www.wordsapi.com/mashape/words/Hell/definitions?when=2024-12-03T05:22:20.276Z&encrypted=8cfdb18ee723919bea9207bfea58bcbbaeb22d0932f891b8

      //paraPhraseApi

      http://147.182.230.132:51230/generate-reply/?option=8&user_input=abc

      // ton changer

      http://147.182.230.132:51230/generate-reply/?option=1&user_input=my

      // checkGrammerURL

      http://147.182.230.132:33230/gram_spell_checker
