App = Ember.Application.create();

App.Store = DS.Store.extend({
  revision: 12,
  adapter: DS.RESTAdapter.create({
    bulkCommit: true, 
  	url: "http://omcmedios.com.ar/geochubut/wp_api/v1",
    serializer: DS.RESTSerializer.extend({
        init: function() {
          this._super();
          this.map('Post', {
              media: { embedded: 'always' }
          });

        },
    }),    
  }),

});

App.Store.registerAdapter('App.Term', DS.RESTAdapter.extend({
  url: "http://omcmedios.com.ar/geochubut/wp_api/v1/taxonomies/category"
}));


Ember.View.reopen({
  didInsertElement: function () {
    this.$().fadeIn(500);
  }
});

App.sesion = Ember.Object.create({
    showSplash: false,
});


