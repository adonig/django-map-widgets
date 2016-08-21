### Django Map Widgets (Beta)
Configurable, pluggable and more user friendly map widgets for Django PostGIS fields.

### Achievements
The aim of the Django map widgets is to make all the Django widgets more user friendly and configurable. Map widgets support major map services (GoogleMaps, OpenStreetMap) for your geoDjango fields.

### Requirments

Django map widgets require the jQuery framework, it does not import a jQuery library. It's working with your global jQuery. If you will use map widgets in Django Admin, you don't need to add a jQuery. Map widgets work with Django Admin jQuery file. 

### PointField Map Widgets

#### Google Map Widget
##### Settings

* **GOOGLE_MAP_API_KEY**: Put your Google API key

* **mapCenterLocationName**: You can give specific location name for center of map. Map widget will found this location coordinates using <a href="https://developers.google.com/maps/documentation/javascript/examples/places-autocomplete" target="_blank"> Google Places Autocomplete</a>. (Optional)

* **mapCenterLocation**: You can give specific coordinates for center of the map. Coordinates must be list type. ([latitude, longitude]) (Optional)

* **zoom** : Default zoom value.

Note: If there is no set value for the map center, (mapCenterLocationName, mapCenterLocation) the widget will be centered by the timezone setting of the project.

Check out this links.

- <a href="https://github.com/erdem/django-map-widgets/blob/master/mapwidgets/constants.py">Timezone Center Locations</a>
- <a href="https://gist.github.com/erdem/8c7d26765831d0f9a8c62f02782ae00d">countries.json</a>


### Usage 

**settings.py**    

    MAP_WIDGETS = {
        "GooglePointFieldWidget": (
            ("zoom", 15),
            ("mapCenterLocationName", "london"),
        ),
        "GOOGLE_MAP_API_KEY": "<google-map-api-key>"
    }

If you want to give specific coordinates for center of the map, you can update your settings file like that. 

    MAP_WIDGETS = {
        "GooglePointFieldWidget": (
            ("zoom", 15),
            ("mapCenterLocation", [57.7177013, -16.6300491]),
        ),
        "GOOGLE_MAP_API_KEY": "<google-map-api-key>"
    }

#### Django Admin
    
    from mapwidgets.widgets import GooglePointFieldWidget
    
    class CityAdmin(admin.ModelAdmin):
    formfield_overrides = {
        models.PointField: {"widget": GooglePointFieldWidget}
    }


#### Django Forms

    from mapwidgets.widgets import GooglePointFieldWidget
    
    class CityAdminForm(forms.ModelForm):
        class Meta:
            model = City
            fields = ("coordinates", "city_hall")
            widgets = {
                'coordinates': GooglePointFieldWidget,
                'city_hall': GooglePointFieldWidget,
            }


#### Preview

![](http://i.imgur.com/QpBycQu.png)

#### Google Map Widget for Django Admin Inlines

As you know Django Admin has inline feature and you can add an inline row with dynamically. In this case, Django default map widget doesn't initialize widget when created a new inline row. 

If you want to use Google Map Widget on admin inlines with no issue, you just need to use `GooglePointFieldInlineWidget` class. 

##### Example

    from mapwidgets.widgets import GooglePointFieldInlineWidget
    
    class DistrictAdminInline(admin.TabularInline):
        model = District
        extra = 3
        formfield_overrides = {
            models.PointField: {"widget": GooglePointFieldInlineWidget}
        }
        
    class CityAdmin(admin.ModelAdmin):
        inlines = (DistrictAdminInline,)


### Google Static Map Widget (ReadOnly)

#### Settings

Django map widgets provide all Google Static Map API features. Check out this <a href="https://developers.google.com/maps/documentation/static-maps/intro" target="_blank">link</a> for google static map features. 

Here is the all default settings attribute for google static map widget.

    MAP_WIDGETS = {
        "GoogleStaticMapWidget": (
            ("zoom", 15),
            ("size", "480x480"),
            ("scale", ""),
            ("format", ""),
            ("maptype", ""),
            ("path", ""),
            ("visible", ""),
            ("style", ""),
            ("language", ""),
            ("region", "")
        ),
    
        "GoogleStaticMapMarkerSettings": (
            ("size", "normal"),
            ("color", ""),
            ("icon", ""),
        )
        "GOOGLE_MAP_API_SIGNATURE": "",
        "GOOGLE_MAP_API_KEY": "",
    }    


##### Usage

If you are not using specific features on Google Static Map API, you just need to update `GOOGLE_MAP_API_KEY` value in your Django settings file. You need also individual size map images, you can pass `size` and `zoom` parameter for each `GoogleStaticMapWidget` class. 


##### Example

**settings.py**

    MAP_WIDGETS = {
        "GoogleStaticMapWidget": (
            ("zoom", 15),
            ("size", "320x320"),
        ),
        "GoogleStaticMapMarkerSettings": (
            ("color", "green"),
        )
        "GOOGLE_MAP_API_KEY": "<google-map-api-key>"
    }

**forms.py**

    from mapwidgets.widgets import GoogleStaticMapWidget
    
    class CityDetailForm(forms.ModelForm):
    
        class Meta:
            model = City
            fields = ("name", "coordinates", "city_hall")
            widgets = {
                'coordinates': GoogleStaticMapWidget,
                'city_hall': GoogleStaticMapWidget(zoom=12, size="240x240"),
            }


### Google Static Map Overlay Widget (ReadOnly)

This widget working with <a href="https://developers.google.com/maps/documentation/static-maps/intro" target="_blank">Magnific Popup</a> jQuery plugin. 

##### Usage

You can use also all static map features in this widget. Besides you can give a `thumbnail_size` value. 
 
Here is the all default settings attribute for google static overlay map widget.

    MAP_WIDGETS = {
        "GoogleStaticMapMarkerSettings": (
            ("size", "normal"),
            ("color", ""),
            ("icon", ""),
        ),
    
        "GoogleStaticOverlayMapWidget": (
            ("zoom", 15),
            ("size", "480x480"),
            ("thumbnail_size", "160x160"),
            ("scale", ""),
            ("format", ""),
            ("maptype", ""),
            ("path", ""),
            ("visible", ""),
            ("style", ""),
            ("language", ""),
            ("region", "")
        ),
    
        "GOOGLE_MAP_API_SIGNATURE": "",
        "GOOGLE_MAP_API_KEY": "",
    }   
 

### Example

    from mapwidgets.widgets import GoogleStaticOverlayMapWidget


    class CityDetailForm(forms.ModelForm):
    
        class Meta:
            model = City
            fields = ("name", "coordinates", "city_hall")
            widgets = {
                'coordinates': GoogleStaticOverlayMapWidget,
                'city_hall': GoogleStaticOverlayMapWidget(zoom=12, thumbnail_size="50x50", size="640x640"),
            }
            

