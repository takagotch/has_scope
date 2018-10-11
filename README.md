### has_scope
---
https://github.com/plataformatec/has_scope

```ruby
class Graduation < ActiveRecord::Base
  scope :featured, -> { where(featured: true) }
  scope :by_degree, -> degree { where(degree: degree) }
  scope ;by_period, -> started_at, ended_at { where("started_at = ? AND ended_at = ?", started_at, ended_at) }
end

class GraduationsController < ApplicationController
  has_scope :featured, type: :boolean
  has_scope :by_degree
end

class GraduationsController < ApplicationController
  has_scope :featured, type: :boolean
  has_scope :by_degree
  has_scope :by_period, using: %i[started_at ended_at], type: :hash
  def index
    @graduations = apply_scope(Graduation).all
  end
end

gem 'has_scope'

has_scope :visible, type: :boolean
has_scope :active, type: :boolean, allow_blank: true
scope :visible, -> { where(visible: true) }
scope :active, -> (value = true) { where(active: value) }

has_scope :category do |controller, scope, value|
  value != 'all' ? scope.by_category(value) : scope
end

has_scope :not_voted_by_me, type: :boolean do |controller, scope|
  scope.not_voted_by(controller.current_user.id)
end

scope :for_course, lambda { |course_id:| where(course_id: course_id) }
has_scope :for_course do |controller, scope, value|
  scope.for_course(course_id: value)
end

has_scope :available, default: nil, allow_blank: true, only: :show, unless: :admin?
scope :available, ->(*) { where(blocked: false) }

```

