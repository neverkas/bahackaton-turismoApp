App.Comment = DS.Model.extend({
  id: DS.attr('number'),
  contentDisplay: DS.attr('string'),
  author: DS.attr('string'),
  date: DS.attr('date'),

  contentHTML: function () {
    return this.get('contentDisplay').htmlSafe();
  }.property('contentDisplay'),

  dateFormated: function () {
    return moment(this.get('date')).format('LLL');
  }.property('date'),
});

App.Media = DS.Model.extend({
  id: DS.attr('number'),
  post: DS.belongsTo('App.Post'),
});

App.Post = DS.Model.extend({
  id: DS.attr('number'),
  title: DS.attr('string'),
  contentDisplay: DS.attr('string'),
  latitude: DS.attr('number'),
  longitude: DS.attr('number'),
  address: DS.attr('string'),
  email: DS.attr('string'),
  phone: DS.attr('string'),
  enabled: DS.attr('boolean'),
  rating: DS.attr('number'),

  commentCount: DS.attr('number'),
  comments: DS.hasMany('App.Comment'),
  
  imageBig: DS.attr('string'),
  imageMedium: DS.attr('string'),
  imageSmall: DS.attr('string'),

  contentHTML: function () {
    return this.get('contentDisplay').htmlSafe();
  }.property('contentDisplay'),

  hasComments: function () {
    return this.get('commentCount') > 0;
  }.property('commentCount'),

  safePhone: function () {
      return this.get('phone').replace(/-/g, '').replace(/\(/g, '').replace(/\)/g, '').replace(/\+/g, '').replace(/\s/g, '');
  }.property('phone'),

});

App.Term = DS.Model.extend({
  id: DS.attr('number'),
  description: DS.attr('string'),
  id: DS.attr('number'),
  idStr: DS.attr('string'),
  name: DS.attr('string'),
  parent: DS.attr('number'),
  parentStr: DS.attr('string'),
  postCount: DS.attr('number'),
  slug: DS.attr('string'),
  taxonomy: DS.attr('string'),
  termTaxonomyId: DS.attr('number'),
  termTaxonomyIdStr: DS.attr('string')
});