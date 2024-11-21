# test7seasol.github.io

// Hello World


// Translater Api Free language transfer api

        https://translate.googleapis.com/translate_a/single?client=gtx&sl=auto&tl=de&dt=t&q=Hello World


// Loti Animation video xml lotianimation view

        implementation(libs.lottie)
    lottie = "6.5.2"
    lottie = { module = "com.airbnb.android:lottie", version.ref = "lottie" }
    
     <com.airbnb.lottie.LottieAnimationView
            android:id="@+id/animationView"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_below="@+id/compassid"
            android:layout_centerHorizontal="true"
            android:padding="@dimen/_75sdp"
            app:lottie_autoPlay="true"
            app:lottie_colorFilter="@color/cardview_dark_background"
            app:lottie_loop="true"
            app:lottie_rawRes="@raw/loadingsplace" />


// QiblaCompassAct quibla compass qubla compass
        
        class QiblaCompassAct : com.digital.compass.direction.Activity.BaseAct<ActivityQiblaCompassBinding>(), SensorEventListener {
            override fun getActivityBinding(inflater: LayoutInflater) =
                ActivityQiblaCompassBinding.inflate(layoutInflater)
        
            private var currentDegree = 0f
            private var qiblaDirection = 0f
        
            private lateinit var fusedLocationClient: FusedLocationProviderClient
            private lateinit var sensorManager: SensorManager
            private var lastAzimuth = 0f
            private var accelerometer: Sensor? = null
            private var magnetometer: Sensor? = null
            private var gravity = FloatArray(3)
            private var geomagnetic = FloatArray(3)
            private var azimuth = 0f
        
            private val startForResult =
                registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
                    if (result.resultCode == RESULT_OK) {
                        val data: Intent? = result.data
                        val selectedCompass = data?.getStringExtra("from")
                        bind.compassImage.setImageResource(getItemsList().find { it.id == selectedCompass }!!.item)
                    }
                }
        
            @SuppressLint("DefaultLocale")
            private fun updateCompassUI(direction: Float) {
                // Rotate the compass image
                val rotateAnimation = android.view.animation.RotateAnimation(
                    currentDegree,
                    direction,
                    android.view.animation.Animation.RELATIVE_TO_SELF,
                    0.5f,
                    android.view.animation.Animation.RELATIVE_TO_SELF,
                    0.5f
                )
                rotateAnimation.duration = 500
                rotateAnimation.fillAfter = true
        
        //        findViewById<ImageView>(R.id.compassImageView).startAnimation(rotateAnimation)
                currentDegree = direction
        
                // Update the direction text
                val cardinalDirection = getCardinalDirection(direction)
                bind.tvid.text = String.format("%.0f° %s", direction, cardinalDirection)
            }
        
            private fun getCardinalDirection(degree: Float): String {
                return when {
                    degree >= 337.5 || degree < 22.5 -> "N"
                    degree >= 22.5 && degree < 67.5 -> "NE"
                    degree >= 67.5 && degree < 112.5 -> "E"
                    degree >= 112.5 && degree < 157.5 -> "SE"
                    degree >= 157.5 && degree < 202.5 -> "S"
                    degree >= 202.5 && degree < 247.5 -> "SW"
                    degree >= 247.5 && degree < 292.5 -> "W"
                    degree >= 292.5 && degree < 337.5 -> "NW"
                    else -> ""
                }
            }
        
            override fun initUI() {
                // Initialize the SensorManager and sensors
                sensorManager = getSystemService(SENSOR_SERVICE) as SensorManager
                accelerometer = sensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER)
                magnetometer = sensorManager.getDefaultSensor(Sensor.TYPE_MAGNETIC_FIELD)
        
        
                bind.toolbarid.apply {
                    tvtoolname.text =
                        getString(R.string.qibla_compass)
        
                    backid.onClick {
                        finish()
                    }
                }
        
                fusedLocationClient = LocationServices.getFusedLocationProviderClient(this)
        
                bind.apply {
                    typeid.onClick {
                        startForResult.launch(
                            Intent(
                                this@QiblaCompassAct, CompassSelectionAct::class.java
                            )
                        )
                    }
                }
        
                bind.tvid.visibility = View.VISIBLE
        
                requestLocationPermission()
            }
        
            @SuppressLint("SetTextI18n")
            private fun displayDirection(azimuth: Float) {
                val roundedAzimuth = azimuth.roundToInt()
                val direction = when (roundedAzimuth) {
                    in 0..22, in 338..360 -> "N"
                    in 23..67 -> "NE"
                    in 68..112 -> "E"
                    in 113..157 -> "SE"
                    in 158..202 -> "S"
                    in 203..247 -> "SW"
                    in 248..292 -> "W"
                    in 293..337 -> "NW"
                    else -> "N" // Default case, though not typically needed
                }
                lastAzimuth = azimuth
            }
        
        
            private fun getLocation() {
                if (ActivityCompat.checkSelfPermission(
                        this, ACCESS_FINE_LOCATION
                    ) != PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(
                        this, ACCESS_COARSE_LOCATION
                    ) != PackageManager.PERMISSION_GRANTED
                ) {
                    return
                }
        
                fusedLocationClient.lastLocation.addOnSuccessListener { location: Location? ->
                    location!!.latitude.toString().log()
                    displayLocationInfo(location)
                }
            }
        
            private fun displayLocationInfo(location: Location) {
                val geocoder = Geocoder(this, Locale.getDefault())
                val addresses = geocoder.getFromLocation(location.latitude, location.longitude, 1)
                addresses?.firstOrNull()?.let {
                    val city = it.locality ?: "Unknown city"
                    val country = it.countryName ?: "Unknown country"
                    ("Location: $city, $country").log()
                    bind.tvlocationclickid.text = "$city, $country"
                }
            }
        
            private fun requestLocationPermission() {
                val locationPermissionRequest = registerForActivityResult(
                    ActivityResultContracts.RequestPermission()
                ) { isGranted ->
                    if (isGranted) {
                        getLocation()
                    } else {
                    }
                }
                locationPermissionRequest.launch(ACCESS_FINE_LOCATION)
            }
        
            override fun onResume() {
                super.onResume()
                accelerometer?.also {
                    sensorManager.registerListener(this, it, SensorManager.SENSOR_DELAY_UI)
                }
                magnetometer?.also {
                    sensorManager.registerListener(this, it, SensorManager.SENSOR_DELAY_UI)
                }
            }
        
            override fun onPause() {
                super.onPause()
                sensorManager.unregisterListener(this)
            }
        
            override fun onSensorChanged(event: SensorEvent) {
        
                if (event == null) return
        
                when (event.sensor.type) {
                    Sensor.TYPE_ACCELEROMETER -> gravity = event.values
                    Sensor.TYPE_MAGNETIC_FIELD -> geomagnetic = event.values
                }
        
                if (gravity != null && geomagnetic != null) {
                    val rotationMatrix = FloatArray(9)
                    val inclinationMatrix = FloatArray(9)
                    if (SensorManager.getRotationMatrix(
                            rotationMatrix, inclinationMatrix, gravity, geomagnetic
                        )
                    ) {
                        val orientation = FloatArray(3)
                        SensorManager.getOrientation(rotationMatrix, orientation)
        
                        val azimuth = Math.toDegrees(orientation[0].toDouble()).toFloat()
                        val adjustedAzimuth = (azimuth + 360) % 360
        
                        // Calculate the difference between azimuth and Qibla direction
                        val directionToQibla = (qiblaDirection - adjustedAzimuth + 360) % 360
        
                        // Update UI (e.g., rotate a compass image)
                        updateCompassUI(directionToQibla)
                    }
                }
        
                when (event.sensor.type) {
                    Sensor.TYPE_ACCELEROMETER -> {
                        gravity[0] = event.values[0]
                        gravity[1] = event.values[1]
                        gravity[2] = event.values[2]
                    }
        
                    Sensor.TYPE_MAGNETIC_FIELD -> {
                        geomagnetic[0] = event.values[0]
                        geomagnetic[1] = event.values[1]
                        geomagnetic[2] = event.values[2]
                    }
                }
        
                val R = FloatArray(9)
                val I = FloatArray(9)
                if (SensorManager.getRotationMatrix(R, I, gravity, geomagnetic)) {
                    val orientation = FloatArray(3)
                    SensorManager.getOrientation(R, orientation)
                    val newAzimuth = Math.toDegrees(orientation[0].toDouble()).toFloat()
        
                    // Rotate compass image
                    bind.compassImage.rotation = -newAzimuth
                    azimuth = newAzimuth
                }
        
                event?.let {
                    when (it.sensor.type) {
                        Sensor.TYPE_ACCELEROMETER -> {
                            System.arraycopy(
                                it.values, 0, accelerometerReading, 0, accelerometerReading.size
                            )
                        }
        
                        Sensor.TYPE_MAGNETIC_FIELD -> {
                            System.arraycopy(it.values, 0, magnetometerReading, 0, magnetometerReading.size)
                        }
                    }
        
                    if (SensorManager.getRotationMatrix(
                            rotationMatrix, null, accelerometerReading, magnetometerReading
                        )
                    ) {
                        SensorManager.getOrientation(rotationMatrix, orientationAngles)
                        val azimuthInDegrees = Math.toDegrees(orientationAngles[0].toDouble()).toFloat()
                        displayDirection(azimuthInDegrees)
                    }
                }
            }
        
            private var accelerometerReading = FloatArray(3)
            private var magnetometerReading = FloatArray(3)
            private var rotationMatrix = FloatArray(9)
            private var orientationAngles = FloatArray(3)
        
            override fun onAccuracyChanged(sensor: Sensor, accuracy: Int) {
                // We won’t be using this method here
            }
        }

    
