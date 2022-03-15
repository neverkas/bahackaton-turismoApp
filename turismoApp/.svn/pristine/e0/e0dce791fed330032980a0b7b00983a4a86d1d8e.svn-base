App.ApplicationView = Ember.View.extend({
  didInsertElement: function () {
    this._super();
    $('.nav').click( function() {
        $('.btn-navbar').addClass('collapsed');
        $('.nav-collapse').removeClass('in').css('height', '0');
    });    
  }
})

App.IndexView = Ember.View.extend({
});

App.RatingView = Ember.View.extend({
  templateName: 'rating',
  tagName: 'ul',
  classNameBindings: [':rating', 'isOneStar:onestar', 'isTwoStart:twostar', 'isThreeStart:threestar', 'isFourStart:fourstar' , 'isFiveStart:fivestar'],

  isOneStar: function () {
    return parseInt(this.get('content')) <= 2;
  }.property('content'),

  isTwoStart: function () {
    return parseInt(this.get('content')) > 2 && parseInt(this.get('content')) <= 4;
  }.property('content'),
  
  isThreeStart: function () {
    return parseInt(this.get('content')) > 4 && parseInt(this.get('content')) <= 6;
  }.property('content'),  

  isFourStart: function () {
    return parseInt(this.get('content')) > 6 && parseInt(this.get('content')) <= 8;
  }.property('content'),  

  isFiveStart: function () {
    return parseInt(this.get('content')) > 8 && parseInt(this.get('content')) <= 10;
  }.property('content'),

  didInsertElement: function () {
    this._super();
  },
});

App.CategorysView = Ember.View.extend({
  didInsertElement: function () {
    this._super();
  }
});

App.PostsView = Ember.View.extend({
  didInsertElement: function () {
    this._super();
  }
});

App.PostView = Ember.View.extend({
  didInsertElement: function () {
    this._super();
  },
});

App.CommentsView = Ember.View.extend({
  didInsertElement: function () {
    this._super();
  }
});



App.LocationView = Ember.View.extend({
  map: null,
  marker: null,

  didInsertElement : function() {
      this._super();
      var _self = this;
      
      var mapOptions = {
          center: new google.maps.LatLng(_self.get('controller.content').get('latitude'), _self.get('controller.content').get('longitude')),
          zoom: 16,
          mapTypeId: google.maps.MapTypeId.ROADMAP
      };
      
      var map = new google.maps.Map(this.$('.bigMap').get(0), mapOptions);
      this.set('map', map);

      google.maps.event.addDomListener(window, 'resize', function() {
          var center = map.getCenter();
          google.maps.event.trigger(map, "resize");
          map.setCenter(center);
      });

      this.setCenter();
  },

  setCenter: function () {

    var map = this.get('map');
    var center = new google.maps.LatLng(this.get('controller.content').get('latitude'), this.get('controller.content').get('longitude'));

    if (this.get('marker'))
        this.get('marker').setMap(null);


    var marker = new google.maps.Marker({
        position: center,
        map: map,
        animation: google.maps.Animation.DROP,
    });    

    map.setCenter(center);
    this.set('marker', marker);

    _self = this;

    var infowindow = new google.maps.InfoWindow();
    if (this.get('map')) {
      google.maps.event.addListener(_self.get('map'), 'click', function() { 
          infowindow.close();
      });
    }
    google.maps.event.addListener(marker, 'click', function() {
      var contentString = '<div id="content">'+
                            '<div id="siteNotice">'+
                            '</div>'+
                            '<h2 id="firstHeading" class="firstHeading">' + _self.get('controller.content').get('title') +'</h2>'+
                            '<div id="bodyContent">'+
                              '<p>'+ _self.get('controller.content').get('address') + '</p>'+
                              '<p><a href="#/post/' + _self.get('controller.content.id') + '" class="btn btn-primary">Ver lugar</a></p>'+
                            '</div>'+
                          '</div>';
      infowindow.setContent(contentString);
      infowindow.open(_self.get('map'), marker);
    });    
  }, 

});


App.PlacesView = Ember.View.extend({
  map: null,
  marker: null,
  limit: 2000000,
  lat: 0,
  long: 0,

  didInsertElement : function() {
      this._super();
      var _self = this;

      if (navigator.geolocation)
      {
        navigator.geolocation.getCurrentPosition(function (position) {
            _self.set('lat', position.coords.latitude);
            _self.set('long', position.coords.longitude);
        });
      }

      var mapOptions = {
          center: new google.maps.LatLng(-34.397, -58.394927199999984),
          zoom: 14,
          mapTypeId: google.maps.MapTypeId.ROADMAP
      };
      
      var map = new google.maps.Map(this.$('.bigMap').get(0), mapOptions);
      this.set('map', map);

      google.maps.event.addDomListener(window, 'resize', function() {
          var center = map.getCenter();
          google.maps.event.trigger(map, "resize");
          map.setCenter(center);
      });      

      this.refreshMarkers();
  },

  setCenter: function () {

    var map = this.get('map');
    var center = new google.maps.LatLng(this.get('lat'), this.get('long'));

    if (this.get('marker'))
        this.get('marker').setMap(null);


    var marker = new google.maps.Marker({
        position: center,
        map: map,
        animation: google.maps.Animation.DROP,
    });    

    map.setCenter(center);
    this.set('marker', marker);



  }.observes('lat', 'long'),

  refreshMarkers: function () {
      var _self = this;

      if (_self.get('lat') != 0 && _self.get('long') != 0) {

        var infowindow = new google.maps.InfoWindow();
        if (this.get('map')) {
          google.maps.event.addListener(_self.get('map'), 'click', function() { 
              infowindow.close();
          });
        }

        var populationOptions = {
          strokeColor: '#3b5998',
          strokeOpacity: 0.8,
          strokeWeight: 2,
          fillColor: '#3b5998',
          fillOpacity: 0.05,
          map: _self.get('map'),
          center: new google.maps.LatLng(_self.get('lat'), _self.get('long')),
          radius: _self.get('limit')
        };

        var cityCircle = new google.maps.Circle(populationOptions);      

        this.get('controller.content').forEach(function (post) {
            var latLngA = new google.maps.LatLng(_self.get('lat'), _self.get('long'));
            var latLngB = new google.maps.LatLng(post.get('latitude'), post.get('longitude'));

            var distance = google.maps.geometry.spherical.computeDistanceBetween (latLngA, latLngB);
            if (distance < _self.get('limit'))
            {
              var marker = new google.maps.Marker({
                  position: latLngB,
                  map: _self.get('map'),
                  animation: google.maps.Animation.DROP,
              });
              google.maps.event.addListener(marker, 'click', function() {
                var contentString = '<div id="content">'+
                                      '<div id="siteNotice">'+
                                      '</div>'+
                                      '<h2 id="firstHeading" class="firstHeading">' + post.get('title') +'</h2>'+
                                      '<div id="bodyContent">'+
                                        '<p>'+ post.get('address') + '</p>'+
                                        '<p><a href="#/post/' + post.get('.id') + '" class="btn btn-primary">Ver lugar</a></p>'+
                                      '</div>'+
                                    '</div>';
                infowindow.setContent(contentString);
                infowindow.open(_self.get('map'), marker);
              });            
            }
        });
      }
  }.observes('controller.content.@each', 'lat', 'long'),
});

App.SplashView = Ember.View.extend({
    percent: 0,
    interval: null,

    progress: function () {
       this.set('percent', this.get('percent') + 15);
       this.$('#progress').attr('style', "width: " + this.get('percent') + "%;");
    },

    willDestroyElement: function () {
       clearInterval(this.get('interval'));
    },

    didInsertElement: function () {
       this._super();
       var self = this;
       this.set("interval", setInterval(function() {self.progress()}, 150));
    }
});