# test7seasol.github.io

            
// Hello World

// Bottom sheet New
                        

                            <style name="full_screen_dialog">
        <item name="android:windowFrame">@null</item>
        <item name="android:windowIsFloating">true</item>
        <item name="android:windowContentOverlay">@null</item>
        <item name="android:windowAnimationStyle">@android:style/Animation.Dialog</item>
        <item name="android:windowSoftInputMode">stateUnspecified|adjustPan</item>
        <item name="android:windowBackground">@android:color/transparent</item>
    </style>
    
                        class DeleteRemoveLockBottomSheetDialog(
                            var activity: Activity,
                            callback: (String) -> Unit
                        ) {
                            var dialog: BottomSheetDialog = BottomSheetDialog(activity,R.style.full_screen_dialog)
                            var binding: DeleteUnlockBottomSheetBinding =
                                DeleteUnlockBottomSheetBinding.inflate(LayoutInflater.from(activity))
                        
                            init {
                                dialog.window?.setBackgroundDrawable(ColorDrawable(Color.TRANSPARENT))
                                // Hide the navigation bar when the dialog appears
                        //        dialog.window?.decorView?.systemUiVisibility =
                        //            (View.SYSTEM_UI_FLAG_IMMERSIVE_STICKY
                        //                    or View.SYSTEM_UI_FLAG_HIDE_NAVIGATION)
                        
                        //        dialog.behavior.isFitToContents = true
                        //        dialog.behavior.peekHeight = BottomSheetBehavior.PEEK_HEIGHT_AUTO // Adjust height dynamically
                        
                                dialog.setContentView(binding.root)
                                dialog.setCancelable(false)
                                dialog.window?.setBackgroundDrawableResource(android.R.color.transparent)
                                dialog.window!!.findViewById<View>(com.google.android.material.R.id.design_bottom_sheet)
                                    .setBackgroundResource(android.R.color.transparent)
                        
                                binding.apply {
                        
                                    tvCancel.onClick {
                                        dialog.dismiss()
                                    }
                                    tvUnLock.onClick {
                                        callback("tvUnLock")
                                        dialog.dismiss()
                                    }
                                    dialog.setOnDismissListener {
                        //                callback("setOnDismiss")
                                        dialog.dismiss()
                                    }
                                }
                                dialog.show()
                            }
                        
                            fun onDismissDialog() {
                                dialog.dismiss()
                            }
                        
                            fun onShowDialog() {
                                dialog.show()
                            }
                        }

            

