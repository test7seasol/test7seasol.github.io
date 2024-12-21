# test7seasol.github.io

// Hello World
        
          buildFeatures {
                viewBinding = true
                buildConfig = true
            }
            
// Custom ext funcations

        
        fun getTimeAgo(pastTimeMillis: Long): String {
            val currentTimeMillis = System.currentTimeMillis()
            val differenceMillis = currentTimeMillis - pastTimeMillis
        
            val seconds = differenceMillis / 1000
            val minutes = seconds / 60
            val hours = minutes / 60
            val days = hours / 24
        
            return when {
                seconds < 60 -> "$seconds seconds ago"
                minutes < 60 -> "$minutes minutes ago"
                hours < 24 -> "$hours hours ago"
                else -> "$days days ago"
            }
        }

    //Save image saveBitmap sharebitmap 

        fun shareImageFromUrl(context: Context, imageUrl: String) {
            CoroutineScope(Dispatchers.IO).launch {
                try {
                    // Download the image as Bitmap
                    val bitmap = Glide.with(context).asBitmap().load(imageUrl).submit().get()
        
                    // Save the Bitmap locally
                    val file = saveBitmapToFile(context, bitmap)
        
                    // Share the image using FileProvider
                    file?.let { shareImage(context, it) }
                } catch (e: Exception) {
                    e.printStackTrace()
                }
            }
        }
        
        private fun saveBitmapToFile(context: Context, bitmap: Bitmap): File? {
            return try {
                // Create a directory in the cache folder
                val cachePath = File(context.cacheDir, "images")
                cachePath.mkdirs() // Ensure the directory exists
        
                // Save the image to a file
                val file = File(cachePath, "shared_image.png")
                val fileOutputStream = FileOutputStream(file)
                bitmap.compress(Bitmap.CompressFormat.PNG, 100, fileOutputStream)
                fileOutputStream.close()
                file
            } catch (e: Exception) {
                e.printStackTrace()
                null
            }
        }
        
        private fun shareImage(context: Context, file: File) {
            // Get the content URI using FileProvider
            val uri: Uri = FileProvider.getUriForFile(context, "${context.packageName}.fileprovider", file)
        
            // Create a sharing intent
            val shareIntent = Intent(Intent.ACTION_SEND).apply {
                type = "image/png"
                putExtra(Intent.EXTRA_STREAM, uri)
                addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION) // Grant permission to the receiving app
            }
        
            // Start the sharing activity
            context.startActivity(Intent.createChooser(shareIntent, "Share Image"))
        }
        
        
        @SuppressLint("NewApi")
        fun saveBitmapToDownloads(context: Context, bitmap: Bitmap, fileName: String) {
            val downloadsDir = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.Q) {
                // For Android 10 and above
                MediaStore.Downloads.EXTERNAL_CONTENT_URI
            } else {
                Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOWNLOADS)
            }
        
            try {
                val contentValues = ContentValues().apply {
                    put(MediaStore.MediaColumns.DISPLAY_NAME, "$fileName.png") // Add extension
                    put(MediaStore.MediaColumns.MIME_TYPE, "image/png")
                    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.Q) {
                        put(MediaStore.MediaColumns.RELATIVE_PATH, Environment.DIRECTORY_DOWNLOADS)
                    }
                }
        
                val uri =
                    context.contentResolver.insert(MediaStore.Downloads.EXTERNAL_CONTENT_URI, contentValues)
                uri?.let {
                    val outputStream: OutputStream? = context.contentResolver.openOutputStream(it)
                    outputStream?.use { stream ->
                        bitmap.compress(Bitmap.CompressFormat.PNG, 100, stream) // Save as PNG
                        Toast.makeText(context, "Saved to Downloads folder", Toast.LENGTH_SHORT).show()
                    }
                }
            } catch (e: Exception) {
                e.printStackTrace()
                Toast.makeText(context, "Failed to save image: ${e.message}", Toast.LENGTH_LONG).show()
            }
        }
        
        fun downloadAndSaveBitmap(context: Context, imageUrl: String, fileName: String) {
            Glide.with(context).asBitmap().load(imageUrl)
                .into(object : com.bumptech.glide.request.target.CustomTarget<Bitmap>() {
                    override fun onResourceReady(
                        resource: Bitmap,
                        transition: com.bumptech.glide.request.transition.Transition<in Bitmap>?
                    ) {
                        saveBitmapToDownloads(context, resource, fileName)
                    }
        
                    override fun onLoadCleared(placeholder: android.graphics.drawable.Drawable?) {
                        // Handle cleanup if needed
                    }
        
                    override fun onLoadFailed(errorDrawable: android.graphics.drawable.Drawable?) {
                        Toast.makeText(context, "Failed to download image", Toast.LENGTH_SHORT).show()
                    }
                })
        }


    // share text
    //copytext
    
    fun shareText(context: Context, textToShare: String) {
        val intent = Intent(Intent.ACTION_SEND).apply {
            type = "text/plain"
            putExtra(Intent.EXTRA_TEXT, textToShare)
        }
    
        // Start the share intent
        val chooser = Intent.createChooser(intent, "Share using")
        context.startActivity(chooser)
    }
    
    fun copyTextToClipboard(context: Context, textToCopy: String) {
        val clipboardManager = context.getSystemService(Context.CLIPBOARD_SERVICE) as ClipboardManager
        val clipData = ClipData.newPlainText("Copied Text", textToCopy)
        clipboardManager.setPrimaryClip(clipData)
    
        // Notify the user
        Toast.makeText(context, "Text copied to clipboard", Toast.LENGTH_SHORT).show()
    }
    
    fun Activity.onBackground(onNext: () -> Unit) {
        CoroutineScope(Dispatchers.IO).launch {
            onNext.invoke()
        }
    }
    



// Custom popup dialog popupdialog popupmanager popupwindow manager
        
        class CustomPopupDialog(
            activity: Activity, anchorView: View, listofcat: ArrayList<String>, callBack: (String) -> Unit
        ) {
            private var popupWindow: PopupWindow? = null
        
            init {
                val popupView: PopupdialogsBinding = PopupdialogsBinding.inflate(activity.layoutInflater)
                LayoutInflater.from(activity).inflate(R.layout.popupdialogs, null)
        
                popupWindow = PopupWindow(
                    popupView.root, ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT
                )
        
                popupWindow!!.animationStyle = R.style.PopupAnimation
        
                popupView.root.setCardBackgroundColor(android.graphics.Color.WHITE)
        //        popupWindow!!.setBackgroundDrawable(ColorDrawable(android.graphics.Color.WHITE))
        
                popupWindow!!.isOutsideTouchable = true
                popupWindow!!.isFocusable = true
        
                val marginRightDp = 10
                val marginRightPx = TypedValue.applyDimension(
                    TypedValue.COMPLEX_UNIT_DIP, marginRightDp.toFloat(), activity.resources.displayMetrics
                ).toInt()
        
                // Get screen dimensions and anchor view location
                val location = IntArray(2)
                anchorView.getLocationOnScreen(location)
        
                val displayMetrics = DisplayMetrics()
                activity.windowManager.defaultDisplay.getMetrics(displayMetrics)
                val screenWidth = displayMetrics.widthPixels
        
                // Calculate the exact x and y positions
                val anchorX = location[0]
                val anchorY = location[1] + anchorView.height
        
                // Determine the x position for the popup with a right margin
                val popupWidth = popupWindow!!.contentView.measuredWidth
                val xPosition = screenWidth - popupWidth - marginRightPx
        
                // Show the popup at the calculated position
                popupWindow!!.showAtLocation(
                    anchorView, Gravity.NO_GRAVITY, anchorX, anchorY
                )
        
                popupView.apply {
                    rvcatid.adapter = RVDragDropAdapter(activity, listofcat) {
                        callBack.invoke(it)
                        popupWindow!!.dismiss()
                    }
                }
            }
        }
    
    <com.google.android.material.card.MaterialCardView xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:layout_margin="@dimen/_15sdp"
        android:background="@android:color/transparent"
        android:clipChildren="false"
        android:clipToPadding="false"
        android:elevation="0dp"
        app:cardCornerRadius="@dimen/_10sdp"
        app:cardElevation="0dp"
        app:strokeColor="#299E9E9E">
    
        <LinearLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content">
    
            <androidx.recyclerview.widget.RecyclerView
                android:id="@+id/rvcatid"
                android:layout_width="wrap_content"
                android:layout_height="@dimen/_300sdp"
                android:orientation="vertical"
                app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"
                tools:itemCount="10"
                tools:listitem="@layout/rv_dragdrop_layout" />
    
        </LinearLayout>
    </com.google.android.material.card.MaterialCardView>


// Base Fragment BaseFragment
    
    abstract class BaseFragment<VB : ViewBinding> : Fragment() {
        lateinit var bind: VB
    
        override fun onCreateView(
            inflater: LayoutInflater,
            container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View {
            bind = getActivityBinding(layoutInflater)
            return bind.root
        }
    
        override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
            super.onViewCreated(view, savedInstanceState)
            initUI()
        }
    
        abstract fun getActivityBinding(inflater: LayoutInflater): VB
    
        abstract fun initUI()
    
    }



// TypewriterTextView chatgpt responce typewriter printertext printtextview
    
    class TypewriterTextView @JvmOverloads constructor(
        context: Context,
        attrs: AttributeSet? = null,
        defStyle: Int = 0
    ) : AppCompatTextView(context, attrs, defStyle) {
    
        private var textToShow: CharSequence = ""
        private var index = 0
        private var delay: Long = 5 // Delay in ms for each character
        private var onTypingCompleted: (() -> Unit)? = null
    
        private val handler = Handler(Looper.getMainLooper())
    
        private val characterAdder = object : Runnable {
            override fun run() {
                if (index < textToShow.length) {
                    text = textToShow.subSequence(0, index + 1) // Update text with next character
                    index++
                    handler.postDelayed(this, delay)
                } else {
                    onTypingCompleted?.invoke() // Notify when typing is complete
                }
            }
        }
    
        fun setTextWithTypingEffect(text: CharSequence, typingDelay: Long = 5, onComplete: (() -> Unit)? = null) {
            textToShow = text
            index = 0
            onTypingCompleted = onComplete
            this.text = "" // Clear the current text
            handler.removeCallbacks(characterAdder)
            handler.post(characterAdder) // Start typing effect
        }
        }
    
      bind.tvresponceid.setTextWithTypingEffect(
                                itemdata.Content.text, typingDelay = 20
                            ) {
                                Log.d("Typewriter", "Typing completed!")
                            }
