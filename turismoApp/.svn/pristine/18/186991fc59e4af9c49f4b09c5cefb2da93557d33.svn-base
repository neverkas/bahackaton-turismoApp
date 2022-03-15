App.Router.map(function() {
 this.resource('splash');
 this.resource('about');  
 this.resource('categorys');  
 this.resource('places');  
 this.resource('posts', {
    path: '/posts/:category'
 });
 this.resource('post', { path: '/post/:id' }, function() {
    this.route('comments');
    this.route('location');
    this.route('media');
  });  
});

App.IndexRoute = Ember.Route.extend({
    redirect: function () {
      if (!App.get('sesion').get('showSplash'))
        this.transitionTo('splash');
    }
});

App.SplashRoute = Ember.Route.extend({
    enter: function () {
        var widthOrHeight = $(window).height() > $(window).width() ? 'width' : 'height';
        $('#splash-content').find('img').css(widthOrHeight, '100%');
        $('#splash').fadeIn();

        Ember.run.later(this, function () {
            $('#splash').fadeOut().detach();
            App.get('sesion').set('showSplash', true);
            this.transitionTo('index');
        }, 1500);
    }
});

App.CategorysRoute = Ember.Route.extend({
    model: function () {
      return App.Term.find();
    },
});

App.PlacesRoute = Ember.Route.extend({
    model: function () {
      return App.Post.find();
    },
});


App.PostsRoute = Ember.Route.extend({
    model: function (param) {
      return App.Post.find({cat: param});
    },

    serialize: function(model, params) {
      return {category: model.get('id')};
    }
});

App.PostRoute = Ember.Route.extend({
    model: function (param) {
      return App.Post.find(param.id);
    },

    serialize: function(model, params) {
      return {id: model.get('id')};
    }
});

App.PostLocationRoute = Ember.Route.extend({
  
  model: function (param, l) {
    return App.Post.find(l.params.id);
  },

  enter: function() {
    var controller = this.controllerFor('post');
    controller.set('showComments', true);
  },

  exit: function () {
    var controller = this.controllerFor('post');
    controller.set('showComments', false);
  },

  renderTemplate: function() {
    this.render('location', {
      into: 'post',
      outlet: 'metaContent'
    });
  }
});

App.PostCommentsRoute = Ember.Route.extend({
  model: function (param, l) {
    return App.Comment.find({post_id: l.params.id});
  },

  enter: function() {
    var controller = this.controllerFor('post');
    controller.set('showComments', true);
  },

  exit: function () {
    var controller = this.controllerFor('post');
    controller.set('showComments', false);
  },

  renderTemplate: function() {
    this.render('comments', {
      into: 'post',
      outlet: 'metaContent'
    });
  }
});
