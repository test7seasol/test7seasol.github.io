# test7seasol.github.io


  // Show First App in Top in display overlay permission

           Hello World
                   android:label="#appname"

                           <activity
            android:name=".activities.SplashActivity"
            android:exported="true"
            android:label="@string/app_name"



          <activity
            android:name=".activities.TranslucentActivity"
            android:configChanges="orientation"
            android:exported="false"
            android:screenOrientation="portrait"
            android:theme="@style/AppTheme.TransparentWindow" />


                <style name="AppTheme.TransparentWindow" parent="@style/Theme.AppCompat.DayNight.NoActionBar">
        <item name="android:windowBackground">@android:color/transparent</item>
        <item name="android:windowNoTitle">true</item>
        <item name="android:windowIsFloating">false</item>
        <item name="android:windowIsTranslucent">true</item>
        <item name="android:windowContentOverlay">@null</item>
        <item name="android:backgroundDimEnabled">false</item>
        <item name="android:windowDisablePreview">true</item>
        <item name="android:colorBackgroundCacheHint">@null</item>
        <item name="android:statusBarColor">#aa000000</item>
        <item name="backgroundColor">@android:color/transparent</item>
    </style>



      val intent = Intent(
                Settings.ACTION_MANAGE_OVERLAY_PERMISSION, Uri.parse("package:$packageName")
            )
            startActivityForResult(intent, REQUEST_OVERLAY_PERMISSION)
            Timer().schedule(object : TimerTask() {
                override fun run() {
                    startActivity(
                        Intent(
                            this@SetDefaultActivity, TranslucentActivity::class.java
                        ).setFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP).putExtra(
                            "autostart",
                            resources.getString(com.trucontactsapp.callerdailer.R.string.allow_displaying_over_other_apps)
                        )
                    )
                }
            }, 50L)
                
    public class TranslucentActivity extends AppCompatActivity {
        private RelativeLayout relLay;
    
        @Override
        public void onCreate(Bundle bundle) {
            super.onCreate(bundle);
            setContentView(R.layout.translucent_activity);
    
            Log.wtf("FATZ", "Come in Heree Trasla");
    
            this.relLay = (RelativeLayout) findViewById(R.id.translayRel);
            TextView textView = (TextView) findViewById(R.id.text_trans);
            this.relLay.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    finish();
                }
            });
    
            String stringExtra = getIntent().getStringExtra("autostart");
            if (stringExtra != null) {
                textView.setText(stringExtra);
            }
        }
    }

    
              <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:id="@+id/translayRel"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    
        <RelativeLayout
            android:id="@+id/topLayout"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_above="@+id/lout_bottom"
            android:background="#22000000" />
    
        <LinearLayout
            android:id="@+id/lout_bottom"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_alignParentBottom="true"
            android:background="@color/white"
            android:orientation="vertical"
            android:paddingTop="@dimen/_6sdp"
            android:paddingBottom="@dimen/_8sdp">
    
            <TextView
                android:id="@+id/text_trans"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="@dimen/_8sdp"
                android:padding="@dimen/_5sdp"
                android:text="Find Contact and allow permission"
                android:textColor="@color/black"
                android:textSize="@dimen/_12sdp" />
    
            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="@dimen/_4sdp"
                android:orientation="horizontal"
                android:padding="@dimen/_8sdp">
    
                <androidx.appcompat.widget.AppCompatImageView
                    android:id="@+id/appIcon"
                    android:layout_width="@dimen/_32sdp"
                    android:layout_height="@dimen/_32sdp"
                    android:layout_gravity="center"
                    android:src="@mipmap/ic_launcher" />
    
                <TextView
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_gravity="center"
                    android:layout_marginLeft="@dimen/_12sdp"
                    android:layout_weight="1"
                    android:text="#Contacts"
                    android:textColor="@color/black"
                    android:textSize="@dimen/_16sdp" />
    
                <com.airbnb.lottie.LottieAnimationView
                    android:id="@+id/animation_view"
                    android:layout_width="@dimen/_50sdp"
                    android:layout_height="@dimen/_50sdp"
                    app:lottie_autoPlay="true"
                    app:lottie_loop="true"
                    app:lottie_rawRes="@raw/animation" />
    
            </LinearLayout>
        </LinearLayout>
    </RelativeLayout>


// Firebase Error in menifesterror and release apk and bundl time menifest error 

        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"

      <property
            android:name="android.adservices.AD_SERVICES_CONFIG"
            android:resource="@xml/gma_ad_services_config"
            tools:replace="android:resource" />


              <meta-data
            android:name="com.google.android.gms.ads.flag.OPTIMIZE_INITIALIZATION"
            android:value="true" />
        <meta-data
            android:name="com.google.android.gms.ads.flag.OPTIMIZE_AD_LOADING"
            android:value="true" />
            
