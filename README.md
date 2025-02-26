# test7seasol.github.io

            
// Hello World

// FastScrollRecyclerView & fastscroll recycler view

                        
                <com.gallery.photos.editpic.Views.FastScrollRecyclerView
                    android:id="@+id/recyclerViewPictures"
                    android:layout_below="@+id/rlToolbar"
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:layout_above="@+id/picturesselectedcontainerid"
                    app:bubbleColor="@color/blue_color_picker"
                    app:bubbleTextColor="@color/white"
                    app:handleColor="#999999"
                    app:spanCount="3"
                    app:layoutManager="androidx.recyclerview.widget.GridLayoutManager"
                    tools:listitem="@layout/item_media" />
                    
            class FastScrollRecyclerView : RecyclerView {
            
                private val fastScroller: FastScroller
            
                constructor(context: Context) : super(context) {
                    fastScroller = context.layout()
                    layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)
                }
            
                @JvmOverloads
                constructor(context: Context, attrs: AttributeSet?, defStyleAttr: Int = 0) : super(context, attrs, defStyleAttr) {
                    fastScroller = context.layout(attrs)
                }
            
                /**
                 * Set a new [RecyclerView.Adapter] that implements [FastScroller.SectionIndexer]
                 * to provide child views for the RecyclerView and section text for the FastScroller.
                 *
                 * @param adapter The new [RecyclerView.Adapter] to set, or null to set none
                 */
                override fun setAdapter(adapter: Adapter<*>?) = super.setAdapter(adapter).also {
                    when (adapter) {
                        is FastScroller.SectionIndexer -> fastScroller.setSectionIndexer(adapter)
                        null -> fastScroller.setSectionIndexer(null)
                    }
                }
            
                /**
                 * Set the visibility state of this view.
                 *
                 * @param visibility One of [View.VISIBLE], [View.INVISIBLE], or [View.GONE]
                 */
                override fun setVisibility(visibility: Int) = super.setVisibility(visibility).also {
                    fastScroller.visibility = visibility
                }
            
                override fun onAttachedToWindow() = super.onAttachedToWindow().also {
                    fastScroller.attachRecyclerView(this)
                }
            
                override fun onDetachedFromWindow() {
                    fastScroller.detachRecyclerView()
                    super.onDetachedFromWindow()
                }
            
                /**
                 * Set a new [FastScroller.FastScrollListener] that will listen to fast scroll events.
                 *
                 * @param fastScrollListener The new [FastScroller.FastScrollListener] to set, or null to set none
                 */
                fun setFastScrollListener(fastScrollListener: FastScrollListener?) =
                    fastScroller.setFastScrollListener(fastScrollListener)
            
                /**
                 * Set a new [FastScroller.SectionIndexer] that provides section text for the [FastScroller].
                 *
                 * @param sectionIndexer The new [FastScroller.SectionIndexer] to set, or null to set no none
                 */
                fun setSectionIndexer(sectionIndexer: FastScroller.SectionIndexer?) =
                    fastScroller.setSectionIndexer(sectionIndexer)
            
                /**
                 * Set the enabled state of fast scrolling.
                 *
                 * @param enabled True to enable fast scrolling, false otherwise
                 */
                fun setFastScrollEnabled(enabled: Boolean) {
                    fastScroller.isEnabled = enabled
                }
            
                /**
                 * Hide the scrollbar when not scrolling.
                 *
                 * @param hideScrollbar True to hide the scrollbar, false to show
                 */
                fun setHideScrollbar(hideScrollbar: Boolean) = fastScroller.setHideScrollbar(hideScrollbar)
            
                /**
                 * Show the scroll track while scrolling.
                 *
                 * @param visible True to show scroll track, false to hide
                 */
                fun setTrackVisible(visible: Boolean) = fastScroller.setTrackVisible(visible)
            
                /**
                 * Set the color of the scroll track.
                 *
                 * @param color The color for the scroll track
                 */
                fun setTrackColor(@ColorInt color: Int) = fastScroller.setTrackColor(color)
            
                /**
                 * Set the color of the scroll handle.
                 *
                 * @param color The color for the scroll handle
                 */
                fun setHandleColor(@ColorInt color: Int) = fastScroller.setHandleColor(color)
            
                /**
                 * Show the section bubble while scrolling.
                 *
                 * @param visible True to show the bubble, false to hide
                 * @param always  True to always show the bubble, false to only show on handle touch
                 */
                @JvmOverloads
                fun setBubbleVisible(visible: Boolean, always: Boolean = false) = fastScroller.setBubbleVisible(visible, always)
            
                /**
                 * Set the background color of the section bubble.
                 *
                 * @param color The background color for the section bubble
                 */
                fun setBubbleColor(@ColorInt color: Int) = fastScroller.setBubbleColor(color)
            
                /**
                 * Set the text color of the section bubble.
                 *
                 * @param color The text color for the section bubble
                 */
                fun setBubbleTextColor(@ColorInt color: Int) = fastScroller.setBubbleTextColor(color)
            
                /**
                 * Set the scaled pixel text size of the section bubble.
                 *
                 * @param size The scaled pixel text size for the section bubble
                 */
                fun setBubbleTextSize(size: Int) = fastScroller.setBubbleTextSize(size)
            
                private fun Context.layout(attrs: AttributeSet? = null) =
                    FastScroller(this, attrs).apply { id = R.id.fast_scroller }
            }

       

