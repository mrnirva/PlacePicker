# PlacePicker

PlacePicker is an alternative library that lets users select a location directly on the map and retrieve its coordinates and address using Geocoder, without relying on Google APIs.

<p align="center"><img src="https://github.com/suchoX/PlacePicker/blob/master/screens/demo.gif"></p>

## Adding PlacePicker to your project

You can simply copy and paste all the pages in their place.

```
dependencies {
  implementation "com.google.android.gms:play-services-maps:18.2.0"
  implementation 'com.google.android.libraries.places:places:5.0.0'
}
```

## How to use

1. You need a Maps API key and add it to your app. [Here's How](https://developers.google.com/maps/documentation/android-sdk/signup)
2. To start The `PlacePickerActivity`:

``` java
val intent = PlacePicker.IntentBuilder()
                .setLatLong(40.748672, -73.985628)  // Initial Latitude and Longitude the Map will load into
                .showLatLong(true)  // Show Coordinates in the Activity
                .setMapZoom(12.0f)  // Map Zoom Level. Default: 14.0
                .setAddressRequired(true) // Set If return only Coordinates if cannot fetch Address for the coordinates. Default: True
                .hideMarkerShadow(true) // Hides the shadow under the map marker. Default: False
                .setMarkerDrawable(R.drawable.marker) // Change the default Marker Image
                .setMarkerImageImageColor(R.color.colorPrimary)
                .setFabColor(R.color.fabColor)
                .setPrimaryTextColor(R.color.primaryTextColor) // Change text color of Shortened Address
                .setSecondaryTextColor(R.color.secondaryTextColor) // Change text color of full Address
                .setBottomViewColor(R.color.bottomViewColor) // Change Address View Background Color (Default: White)
                .setMapRawResourceStyle(R.raw.map_style)  //Set Map Style (https://mapstyle.withgoogle.com/)
                .setMapType(MapType.NORMAL)
                .setPlaceSearchBar(true, GOOGLE_API_KEY) //Activate GooglePlace Search Bar. Default is false/not activated. SearchBar is a chargeable feature by Google
                .onlyCoordinates(true)  //Get only Coordinates from Place Picker
                .hideLocationButton(true)   //Hide Location Button (Default: false)
                .disableMarkerAnimation(true)   //Disable Marker Animation (Default: false)
                .build(this)
            startActivityForResult(intent, Constants.PLACE_PICKER_REQUEST)
```
3. Then get the data `onActivityResult`
```java
override fun onActivityResult(requestCode: Int,resultCode: Int,data: Intent?) {
        if (requestCode == Constants.PLACE_PICKER_REQUEST) {
            if (resultCode == Activity.RESULT_OK) {
            val addressData = data?.getParcelableExtra<AddressData>(Constants.ADDRESS_INTENT)
            }
        } else {
            super.onActivityResult(requestCode, resultCode, data)
        }
    }
```
Note: Placepicker DOES NOT access your location. It's your app's responsibility to fet and provide the User's location and send via `setLatLong`. The MyLocation button takes the user in the sent location

Note: `PlacePickerActivity` uses the default theme of your app. If you want to change the theme, declare it in your app's Manifest:
```xml
<activity
    android:name="com.placepicker.PlacePickerActivity"
    android:theme="@style/PlacePickerTheme"/> //Included FullScreen Day-Night Theme
```

If you are using Java instead of Kotlin:
```java
Intent intent = new PlacePicker.IntentBuilder()
                ...
                
                startActivityForResult(intent, Constants.PLACE_PICKER_REQUEST);
                
                ...

@Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        if (requestCode == Constants.PLACE_PICKER_REQUEST) {
            if (resultCode == Activity.RESULT_OK && data != null) {
              AddressData addressData = data.getParcelableExtra(Constants.ADDRESS_INTENT);
            }
        } else {
            super.onActivityResult(requestCode, resultCode, data);
        }
    }
```
