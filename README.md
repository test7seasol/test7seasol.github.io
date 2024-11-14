# test7seasol.github.io

// Hello World


// Google map library googlemap lib locationmap
            
            osmdroidAndroid = "6.1.10"
            
            osmdroid-android = { module = "org.osmdroid:osmdroid-android", version.ref = "osmdroidAndroid" }
            
            
                implementation(libs.osmdroid.android)
            
                play-services-location = { module = "com.google.android.gms:play-services-location", version.ref = "playServicesLocation" }
            playServicesLocation = "21.3.0"
        
        
            <org.osmdroid.views.MapView
                android:id="@+id/osmMapView"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />
        
        
                class MapViewActivity : BaseAct<ActivityMapViewBinding>() {
            private lateinit var mapView: MapView
            private lateinit var fusedLocationClient: FusedLocationProviderClient
        
            override fun getActivityBinding(inflater: LayoutInflater) =
                ActivityMapViewBinding.inflate(layoutInflater)
        
            @SuppressLint("SetTextI18n")
            override fun initUI() {
                bind.toolid.apply {
                    backid.onClick {
                        finish()
                    }
                    tvtoolname.text = "Map View"
                }
        
                Configuration.getInstance().load(
                    applicationContext,
                    applicationContext.getSharedPreferences("osm_prefs", MODE_PRIVATE)
                )
        
                // Initialize MapView and LocationClient
                mapView = findViewById(R.id.osmMapView)
                mapView.setMultiTouchControls(true)
                fusedLocationClient = LocationServices.getFusedLocationProviderClient(this)
                // Request location permission and get current location
                requestLocationPermission()
            }
        
            private fun requestLocationPermission() {
                if (ActivityCompat.checkSelfPermission(
                        this,
                        Manifest.permission.ACCESS_FINE_LOCATION
                    ) != PackageManager.PERMISSION_GRANTED
                ) {
                    ActivityCompat.requestPermissions(
                        this,
                        arrayOf(Manifest.permission.ACCESS_FINE_LOCATION),
                        1
                    )
                } else {
                    getCurrentLocation()
                }
            }
        
            override fun onRequestPermissionsResult(
                requestCode: Int,
                permissions: Array<out String>,
                grantResults: IntArray
            ) {
                super.onRequestPermissionsResult(requestCode, permissions, grantResults)
                if (requestCode == 1 && grantResults.isNotEmpty() && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    getCurrentLocation()
                }
            }
        
            private fun getCurrentLocation() {
                if (ActivityCompat.checkSelfPermission(
                        this,
                        Manifest.permission.ACCESS_FINE_LOCATION
                    ) != PackageManager.PERMISSION_GRANTED
                ) {
                    return
                }
        
                fusedLocationClient.lastLocation.addOnSuccessListener { location: Location? ->
                    location?.let {
                        val currentGeoPoint = GeoPoint(it.latitude, it.longitude)
                        showLocationOnMap(currentGeoPoint)
                    }
                }
            }
        
            private fun showLocationOnMap(location: GeoPoint) {
                // Set map controller and zoom to the current location
                val mapController: IMapController = mapView.controller
                mapController.setZoom(15.0)
                mapController.setCenter(location)
        
                // Add a marker to indicate the current location
                val marker = Marker(mapView)
                marker.position = location
                marker.title = "You are here"
                marker.setAnchor(Marker.ANCHOR_CENTER, Marker.ANCHOR_BOTTOM)
                mapView.overlays.add(marker)
                mapView.invalidate()
            }
        
            override fun onResume() {
                super.onResume()
                mapView.onResume()
            }
        
            override fun onPause() {
                super.onPause()
                mapView.onPause()
            }
        
            override fun onDestroy() {
                super.onDestroy()
                mapView.onDetach()
            }
        }




// Compass Logic android compassview compass 
        
        class QiblaCompassAct : BaseAct<ActivityQiblaCompassBinding>(), SensorEventListener {
            override fun getActivityBinding(inflater: LayoutInflater) =
                ActivityQiblaCompassBinding.inflate(layoutInflater)
        
            private lateinit var fusedLocationClient: FusedLocationProviderClient
            private lateinit var sensorManager: SensorManager
            private var lastAzimuth = 0f
        
            private var accelerometer: Sensor? = null
            private var magnetometer: Sensor? = null
            private val gravity = FloatArray(3)
            private val geomagnetic = FloatArray(3)
            private var azimuth = 0f
        
            override fun initUI() {
                // Initialize the SensorManager and sensors
                sensorManager = getSystemService(SENSOR_SERVICE) as SensorManager
                accelerometer = sensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER)
                magnetometer = sensorManager.getDefaultSensor(Sensor.TYPE_MAGNETIC_FIELD)
        
                bind.toolbarid.apply {
                    tvtoolname.text = "Qibla compass"
                    backid.onClick {
                        finish()
                    }
                }
        
                fusedLocationClient = LocationServices.getFusedLocationProviderClient(this)
        
                bind.apply {
                    typeid.onClick {
        
                    }
                }
        
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
                bind.tvid.text = "$roundedAzimuth° - $direction"
                lastAzimuth = azimuth
            }
        
        
            private fun getLocation() {
                if (ActivityCompat.checkSelfPermission(
                        this,
                        ACCESS_FINE_LOCATION
                    ) != PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(
                        this,
                        ACCESS_COARSE_LOCATION
                    ) != PackageManager.PERMISSION_GRANTED
                ) {
                    return
                }
        
                fusedLocationClient.lastLocation.addOnSuccessListener { location: Location? ->
                    location!!.latitude.toString().log()
                    location?.let {
                        displayLocationInfo(it)
                    }
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
                                it.values,
                                0,
                                accelerometerReading,
                                0,
                                accelerometerReading.size
                            )
                        }
        
                        Sensor.TYPE_MAGNETIC_FIELD -> {
                            System.arraycopy(it.values, 0, magnetometerReading, 0, magnetometerReading.size)
                        }
                    }
        
                    if (SensorManager.getRotationMatrix(
                            rotationMatrix,
                            null,
                            accelerometerReading,
                            magnetometerReading
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


// Current location get current location
        
        class MapLocationActivity : BaseAct<ActivityMapLocationBinding>() {
            override fun getActivityBinding(inflater: LayoutInflater) =
                ActivityMapLocationBinding.inflate(layoutInflater)
        
            private lateinit var fusedLocationClient: FusedLocationProviderClient
        
            override fun initUI() {
                fusedLocationClient = LocationServices.getFusedLocationProviderClient(this)
        
                bind.toolid.apply {
                    backid.onClick {
                        finish()
                    }
                    tvtoolname.text = "Map Location"
                }
        
                bind.apply {
                    btnid.gone()
                    btnshareid.gone()
                    btnid.onClick {
                        startActivityWithBundle<MapViewActivity>()
                    }
        
                    btnshareid.onClick {
                        val detail =
                            "Location: ${tvaddressid.text.toString()} \n " + "Latitude: ${latitudeid.text.toString()} \n " + "Longitude: ${longtitudeid.text.toString()} \n " + "Altitude: ${altitudeid.text.toString()} \n " + "Speed: ${speedid.text.toString()} \n "
        
                        shareDetails(detail)
                    }
                }
                requestLocationPermission()
            }
        
            private fun shareDetails(details: String) {
                val shareIntent = Intent().apply {
                    action = Intent.ACTION_SEND
                    putExtra(Intent.EXTRA_TEXT, details)
                    type = "text/plain"
                }
                startActivity(Intent.createChooser(shareIntent, "Share details via"))
            }
        
            private fun requestLocationPermission() {
                val locationPermissionRequest = registerForActivityResult(
                    ActivityResultContracts.RequestPermission()
                ) { isGranted ->
                    if (isGranted) {
                        startLocationUpdates()
                    } else {
                        "Location permission is required to display location data.".tos(this)
                    }
                }
                locationPermissionRequest.launch(ACCESS_FINE_LOCATION)
            }
        
            private fun startLocationUpdates() {
                if (ActivityCompat.checkSelfPermission(
                        this, ACCESS_FINE_LOCATION
                    ) != PackageManager.PERMISSION_GRANTED
                ) {
                    return
                }
        
                val locationRequest = LocationRequest.create().apply {
                    interval = 10000 // Update interval in milliseconds
                    fastestInterval = 5000 // Fastest update interval in milliseconds
                    priority = LocationRequest.PRIORITY_HIGH_ACCURACY
                }
                fusedLocationClient.requestLocationUpdates(locationRequest, locationCallback, null)
            }
        
            private val locationCallback = object : LocationCallback() {
                override fun onLocationResult(locationResult: LocationResult) {
                    locationResult.lastLocation?.let { location ->
                        displayLocationInfo(location)
                    }
                }
            }
        
            private fun displayLocationInfo(location: Location) {
                val latitude = location.latitude
                val longitude = location.longitude
                val altitude = location.altitude
                val speed = location.speed
        
                // Get address using Geocoder
                val geocoder = Geocoder(this, Locale.getDefault())
                val address = try {
                    val addresses = geocoder.getFromLocation(latitude, longitude, 1)
                    addresses?.get(0)?.getAddressLine(0) ?: "Address not available"
                } catch (e: Exception) {
                    "Address not available"
                }
        
                // Display information
                bind.latitudeid.text = latitude.toString()
                bind.tvaddressid.text = address.toString()
                bind.longtitudeid.text = longitude.toString()
                bind.altitudeid.text = "$altitude meters"
                bind.speedid.text = "${speed} m/s"
                bind.btnid.visible()
                bind.btnshareid.visible()
            }
        
            override fun onStop() {
                super.onStop()
                // Stop location updates when activity is no longer visible
                fusedLocationClient.removeLocationUpdates(locationCallback)
            }
        
            private fun openLocationInMap(latitude: Double, longitude: Double) {
                val uri = "geo:$latitude,$longitude?q=$latitude,$longitude(Current Location)"
                val mapIntent = Intent(Intent.ACTION_VIEW, Uri.parse(uri))
                mapIntent.setPackage("com.google.android.apps.maps")
                if (mapIntent.resolveActivity(packageManager) != null) {
                    startActivity(mapIntent)
                } else {
                    // Handle case where Google Maps is not installed
                    ("Google Maps is not installed.").tos(this)
                }
            }
        }


// weather data waither data weather data
        
        class WeatherActivity : BaseAct<ActivityWeatherBinding>() {
            override fun getActivityBinding(inflater: LayoutInflater) =
                ActivityWeatherBinding.inflate(layoutInflater)
        
            private lateinit var fusedLocationClient: FusedLocationProviderClient
        
            private val LOCATION_PERMISSION_REQUEST_CODE = 1
        
            override fun initUI() {
                bind.toolbarid.apply {
                    backid.onClick {
                        finish()
                    }
                    tvtoolname.text = "Weather"
                }
        
                val dateFormat = SimpleDateFormat("yyyy-MM-dd HH:mm", Locale.getDefault())
                val currentDateAndTime: String = dateFormat.format(Date())
        
                bind.apply {
                    tvdatetimeid.text = currentDateAndTime
                }
        
                fusedLocationClient = LocationServices.getFusedLocationProviderClient(this)
        
                checkLocationPermissionAndFetchTemperature()
        
            }
        
        
            private fun checkLocationPermissionAndFetchTemperature() {
                if (ContextCompat.checkSelfPermission(
                        this, ACCESS_FINE_LOCATION
                    ) == PackageManager.PERMISSION_GRANTED
                ) {
                    fetchLocationAndTemperature()
                } else {
                    ActivityCompat.requestPermissions(
                        this, arrayOf(ACCESS_FINE_LOCATION), LOCATION_PERMISSION_REQUEST_CODE
                    )
                }
            }
        
            override fun onRequestPermissionsResult(
                requestCode: Int, permissions: Array<String>, grantResults: IntArray
            ) {
                super.onRequestPermissionsResult(requestCode, permissions, grantResults)
                if (requestCode == LOCATION_PERMISSION_REQUEST_CODE && grantResults.isNotEmpty() && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    fetchLocationAndTemperature()
                } else {
                    ("Location permission denied").tos(this)
                }
            }
        
            private fun fetchLocationAndTemperature() {
                if (ActivityCompat.checkSelfPermission(
                        this, ACCESS_FINE_LOCATION
                    ) != PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(
                        this, Manifest.permission.ACCESS_COARSE_LOCATION
                    ) != PackageManager.PERMISSION_GRANTED
                ) {
        
                    return
                }
                fusedLocationClient.lastLocation.addOnSuccessListener { location ->
                    if (location != null) {
                        val latitude = location.latitude
                        val longitude = location.longitude
        
                        fetchTemperature(latitude, longitude)
        
                        val geocoder = Geocoder(this, Locale.getDefault())
                        try {
                            val addressList =
                                geocoder.getFromLocation(latitude, longitude, 1) // Get the closest address
                            if (addressList != null && addressList.isNotEmpty()) {
                                val address = addressList[0] // Get the first address
                                val city = address.locality ?: "Unknown City"
                                val country = address.countryName ?: "Unknown Country"
        
                                // Show the address in a TextView
                                val closestArea = "$city, $country"
                                bind.tvlocationid.text = closestArea // Update TextView with location
                            } else {
                                bind.tvlocationid.text = "Unable to retrieve address"
                            }
                        } catch (e: IOException) {
                            e.printStackTrace()
                            bind.tvlocationid.text = "Error retrieving address"
                        }
                    } else {
                        bind.tvlocationid.text = "Unable to retrieve location"
                    }
                }
            }
        
            private fun fetchTemperature(latitude: Double, longitude: Double) {
                // Modify the URL to request both current weather and 3-day forecast
                val url =
                    "https://api.open-meteo.com/v1/forecast?latitude=$latitude&longitude=$longitude&current_weather=true&daily=temperature_2m_max,temperature_2m_min&timezone=auto"
        
                val client = OkHttpClient()
                val request = Request.Builder().url(url).build()
        
                client.newCall(request).enqueue(object : okhttp3.Callback {
                    override fun onFailure(call: okhttp3.Call, e: IOException) {
                        runOnUiThread {
                            // Handle failure case
                            ("Failed to retrieve weather data").tos(this@WeatherActivity)
                        }
                    }
        
                    override fun onResponse(call: okhttp3.Call, response: okhttp3.Response) {
                        if (response.isSuccessful) {
                            response.body?.let { responseBody ->
                                val responseData = responseBody.string()
                                val jsonObject = JSONObject(responseData)
        
                                // Parse the current weather data
                                val currentWeather = jsonObject.getJSONObject("current_weather")
                                val temperature =
                                    currentWeather.getDouble("temperature") // Current temperature
                                val windSpeed = currentWeather.getDouble("windspeed") // Windspeed in km/h
                                val windDirection =
                                    currentWeather.getDouble("winddirection") // Wind direction in °
                                val humidity = currentWeather.optInt("humidity", -1) // Today's humidity
        
                                // Parse the 3-day forecast temperatures
                                val dailyWeather = jsonObject.getJSONObject("daily")
                                val nextThreeDaysTemps = ArrayList<String>()
                                val nextThreeDaysNames = ArrayList<String>()
        
                                val calendar = Calendar.getInstance()
        
        
                                for (i in 1..3) {
                                    calendar.add(Calendar.DAY_OF_YEAR, 1)
                                    val dayName =
                                        SimpleDateFormat("EEEE", Locale.getDefault()).format(calendar.time)
        
                                    val day = dailyWeather.getString("time").split(",")[i]
                                    val maxTemp =
                                        dailyWeather.getJSONArray("temperature_2m_max").getDouble(i)
                                    val minTemp =
                                        dailyWeather.getJSONArray("temperature_2m_min").getDouble(i)
                                    nextThreeDaysTemps.add("$maxTemp")
                                    nextThreeDaysNames.add(dayName)
                                }
        
                                // Log the data (for debugging)
                                Log.d("WeatherData", "Today's Temperature: $temperature°C")
                                Log.d("WeatherData", "Today's Wind Speed: $windSpeed km/h")
                                Log.d("WeatherData", "Today's Wind Direction: $windDirection°")
                                Log.d("WeatherData", "Today's Humidity: $humidity%")
                                nextThreeDaysTemps.forEach {
                                    Log.d("WeatherData", "Next 3 Days: $it")
                                }
        
                                // Update the UI with the parsed data
                                runOnUiThread {
                                    bind.tvtempid.text = "$temperature° C" // Current temperature
                                    bind.tvsectempid.text = "$temperature°"
                                    bind.tvwindid.text = "$windSpeed km/h"
                                    bind.tvhumidityid.text = "$humidity%" // Today's humidity
        
                                    // Display the next 3 days' temperatures
        
                                    (nextThreeDaysTemps.joinToString("\n")).log()
                                    bind.tvtempnextonedayid.text = "${nextThreeDaysTemps[0]}°C"
                                    bind.tvtempnexttwodayid.text = "${nextThreeDaysTemps[1]}°C"
                                    bind.tvtempnextthreedayid.text = "${nextThreeDaysTemps[2]}°C"
        
                                    bind.tvdaynameoneid.text = "${nextThreeDaysNames[0]}"
                                    bind.tvdaynametwoid.text = "${nextThreeDaysNames[1]}"
                                    bind.tvdaynamethreeid.text = "${nextThreeDaysNames[2]}"
        
                                }
                            }
                        } else {
                            runOnUiThread {
                                // Handle response failure
                                ("Failed to retrieve weather data").tos(this@WeatherActivity)
                            }
                        }
                    }
                })
            }
        }

// Ext Funcation
    
    inline fun <reified T : Activity> Context.startActivityWithBundle(bundle: Bundle? = null) {
        val intent = Intent(this, T::class.java).apply {
            bundle?.let { putExtras(it) }
        }
        startActivity(intent)
    }
