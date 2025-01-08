# test7seasol.github.io

       
            
// Hello World
        


// Custom Popupwindow dialog popup dialog popupmenu 

        implementation("com.github.kakajika:RelativePopupWindow:0.4.1")

        
       class CustomPopup(
           private val activity: Activity, private val isTouchableOutside: Boolean = true, // Whether the popup can be dismissed by touching outside
           private val isFocusable: Boolean = true, var onClick: (String) -> Unit
       ) {
           private var popupWindow: RelativePopupWindow? = null
           var binding: PopupdialogBinding = PopupdialogBinding.inflate(LayoutInflater.from(activity))
       
           fun show(anchorView: View, xOffsetDp: Int = 0, yOffsetDp: Int = 0) {
               if (popupWindow == null) {
       
                   popupWindow = RelativePopupWindow(
                       binding.root, ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT, isFocusable
                   ).apply {
                       isOutsideTouchable = isTouchableOutside
                       elevation = TypedValue.applyDimension(
                           TypedValue.COMPLEX_UNIT_DIP, 30f, activity.resources.displayMetrics
                       )
                   }
               }
       
               val xOffsetPx = TypedValue.applyDimension(
                   TypedValue.COMPLEX_UNIT_DIP, xOffsetDp.toFloat(), activity.resources.displayMetrics
               ).toInt()
               val yOffsetPx = TypedValue.applyDimension(
                   TypedValue.COMPLEX_UNIT_DIP, yOffsetDp.toFloat(), activity.resources.displayMetrics
               ).toInt()
       
       //        popupWindow?.showAsDropDown(anchorView, xOffsetPx, yOffsetPx)
       
               popupWindow?.showOnAnchor(
                   anchorView, RelativePopupWindow.VerticalPosition.ABOVE, RelativePopupWindow.HorizontalPosition.RIGHT, xOffsetDp, yOffsetDp, true
               )
       
               binding.apply {
       
                   propertiesid.onClick {
                       onClick.invoke("Properties")
                       popupWindow!!.dismiss()
                   }
               }
           }
       
           fun dismiss() {
               popupWindow?.dismiss()
           }
       }

        binding.moremultiid.onClick {
            val customPopup = CustomPopup(this, onClick = {
                when (it) {
                }
            })
            customPopup.show(binding.moremultiid, 0, 0)
        }
        

       ------ Xml
       <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_centerInParent="true"
    android:layout_margin="@dimen/_12sdp"
    android:clipChildren="false"
    android:elevation="@dimen/_10sdp"
    android:clipToPadding="false">

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_marginEnd="@dimen/_12sdp"
        android:layout_marginBottom="@dimen/_5sdp"
        android:background="@drawable/rounded_shape"
        android:backgroundTint="@color/white"
        android:gravity="center"
        android:elevation="@dimen/_10sdp"
        android:orientation="vertical"
        android:paddingStart="@dimen/_10sdp"
        android:paddingTop="@dimen/_5sdp"
        android:paddingEnd="@dimen/_10sdp"
        android:paddingBottom="@dimen/_5sdp">

        <TextView
            android:id="@+id/propertiesid"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="@dimen/_7sdp"
            android:background="?selectableItemBackground"
            android:text="Properties"
            android:textColor="@color/black"
            android:textSize="@dimen/_12sdp" />

            </LinearLayout?
            </RelativeLayout>
