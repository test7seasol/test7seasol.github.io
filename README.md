# test7seasol.github.io

            
// Hello World

// Pick Images

    private fun showImagePickerDialog() {
        val options = arrayOf(
            getString(R.string.pick_from_gallery), getString(R.string.capture_from_camera)
        )

        AlertDialog.Builder(this).setTitle(getString(R.string.select_an_option))
            .setItems(options) { _, which ->
                when (which) {
                    0 -> {
                            // Fallback to classic picker
                            photoPickerLauncher.launch(
                                PickVisualMediaRequest(
                                    ActivityResultContracts.PickVisualMedia.ImageOnly
                                )
                            )
                    }

                    1 -> openCamera()
                }
            }.setNegativeButton(R.string.cancel, null).show()
    }




    private val photoPickerLauncher =
        registerForActivityResult(ActivityResultContracts.PickVisualMedia()) { uri ->
            uri?.let {
                try {
                    // Create a file in your app's private storage
                    val file = File(
                        getExternalFilesDir(Environment.DIRECTORY_PICTURES),
                        "image_${System.currentTimeMillis()}.jpg"
                    )

                    // Copy the content to your private storage
                    contentResolver.openInputStream(uri)?.use { input ->
                        FileOutputStream(file).use { output ->
                            input.copyTo(output)
                        }
                    }

                    val contentUri = FileProvider.getUriForFile(
                        this, "${packageName}.provider", file
                    )
                    wysiwygEditor?.insertImage(contentUri.toString(), "Selected Image")

                } catch (e: Exception) {
                    Toast.makeText(this, "Failed to save image", Toast.LENGTH_SHORT).show()
                    e.printStackTrace()
                }
            }
        }
        
