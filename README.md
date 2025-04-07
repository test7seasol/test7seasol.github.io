# test7seasol.github.io

            
// Hello World

// Flex Box

    implementation("com.google.android.flexbox:flexbox:3.0.0")
            
            <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:tools="http://schemas.android.com/tools"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                xmlns:flexbox="http://schemas.android.com/apk/res-auto"
                android:orientation="vertical"
                tools:context=".activity.TestActivity">
            
                <com.google.android.flexbox.FlexboxLayout
                    android:id="@+id/flexboxLayout"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:padding="8dp"
                    flexbox:alignItems="center"
                    flexbox:flexWrap="wrap"
                    flexbox:justifyContent="flex_start" />
            
            </LinearLayout>

            ----------------- activity code
            
              val textItems =
                        listOf("Item 1", "Item 2", "Itdfwefwefefem 3", "Item 4", "Ifwefwefewfwetem 5")
            
                    for (text in textItems) {
                        val textView = TextView(this).apply {
                            setText(text)
                            setPadding(20, 10, 20, 10)
                            setTextSize(TypedValue.COMPLEX_UNIT_SP, 16f)
                            setTextColor(Color.WHITE)
                            setBackgroundResource(R.drawable.bg_textview)  // Default blue background
            
                            val params = FlexboxLayout.LayoutParams(
                                ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT
                            ).apply {
                                flexShrink = 1f
                                setMargins(10, 10, 10, 10)
                            }
                            layoutParams = params
            
                            // Click event to change color
                            setOnClickListener {
                                selectedTextView?.setBackgroundResource(R.drawable.bg_textview) // Reset old selection
                                setBackgroundResource(R.drawable.bg_textview_black) // Change clicked item to black
                                selectedTextView = this // Store the selected item
                            }
                        }
                        bind.flexboxLayout.addView(textView)
                        

