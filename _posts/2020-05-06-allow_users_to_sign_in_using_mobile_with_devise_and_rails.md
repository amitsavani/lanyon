---
layout: post
title: How to allow users to sign in using mobile number with Devise and Rails?
article: 2
---
Most people in the world who use smartphones don't own/use email. However they know how to operate the mobile and apps.

What if you need to build an app for such users who want to access your app and it requires some kind of authentication mechanism?

If you are building a web application using rails, the natural choice for authentication would be Devise. Conventionally Device's promotes email to authenticate users. Replacing email with mobile as authentication key needs some tweaks in configurations.

### Prerequisite
Ensure you have configured `root` in your application inside `config/routes.rb`. You can skip this section and jump to **Install Devise** step

If you just created a new rails app, let us create a simple `HomeController` with `index` action for demo purpose. Run following from command line

`rails g controller Home index`

Then change `config/routes.rb` and configure `root`

```ruby
root 'home#index'
```

### Install Devise
You can skip this step if you have already set up the Device.

1. Modify Gemfile
Add `gem 'devise'` to your `Gemfile` and run `bundle install` from the command line

2. Run Devise Generator
Next, run `rails generate devise:install` from the command line

Follow instructions appear in the console to complete configuration.

### Create Model
Create a model for authentication passing `mobile` as additional field

`rails generate devise User mobile`

### Modify Generated Migration
Devise is going to use `mobile` to find matching user in the app so we should add index to `mobile` to make searching faster. This command creates  `User` model, creates a migration for it and changes `routes.rb` to add Devise specific routes.

Open the generated migration and make following changes

1. Mark `mobile` field as not `null`

```ruby
t.string :mobile, null: false
```

Add following line below the the index for `email`

```ruby
add_index :users, :mobile,               unique: true
```

### Run Migration

Run `rails db:migrate` from the command line to create `users` table.


### Enable Authentication
Assuming you want to protect all views/controllers, add following to enable authentication for all controllers in `ApplicationController`

```ruby
before_action :authenticate_user!
```

Now start `rails server` and hit `http://localhost:3000` to verify the setup. You should get a login page.

### Override Devise's Default Views

Devise provides default views for Sign In, Sign Up, Reset Password etc. All those views are configured to use `email` which we need to replace with `mobile`.

1. Copy views from Devise to your app  
Run `rails generate devise:views` from command line

2. Replace `email` with `mobile` in Sign In and Sign Up devise views  
Open `app/views/devise/sessions/new.html.erb` and `app/views/devise/registrations/new.html.erb` to replace `email` with `mobile`  
Also, replace `email_filed` with `telephone_field`. The updated configuration will look like in both views

```ruby
<div class="field">
 <%= f.label :mobile %><br />
 <%= f.telephone_field :mobile, autofocus: true, autocomplete: "mobile" %>
</div>
```
### Whitelist mobile param via Strong parameters
Devise controllers santize parameters to ensure only require parameters are sent and rest are ignored. We added `mobile` field which is not in the whitelist provided by Deviase. We need to add it to ensure it is allowed to use by Devise. Here is how it looks like:

```ruby
class ApplicationController < ActionController::Base
  before_action :configure_permitted_parameters, if: :devise_controller?
  before_action :authenticate_user!

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:mobile])
    devise_parameter_sanitizer.permit(:sign_in, keys: [:mobile])
  end
end
```

Check [Whitelisting Custom Fields](https://github.com/heartcombo/devise#strong-parameters) for more details.


### Override Devise's Default Model Configuration

Add following to your `User` model to avoid `email` as authentication key

```ruby
validates :email, uniqueness: true
validates :mobile, uniqueness: true

# Search user by mobile(not email)
def self.find_first_by_auth_conditions(warden_conditions)
  conditions = warden_conditions.dup
  where(mobile: conditions[:mobile]).first
end

# Stop using email as authentication key
def email_required?
  false
end

def email_changed?
  false
end

def will_save_change_to_email?
  false
end
```

Refer [How To: Allow users to sign in using their username or email address](https://github.com/heartcombo/devise/wiki/How-To:-Allow-users-to-sign-in-using-their-username-or-email-address) For more details

### Change Initializer to support mobile as authentication key

Open `config/initializers/devise.rb` and ensure following configuration:

```ruby
config.authentication_keys = [:mobile]
config.case_insensitive_keys = [:mobile]
config.strip_whitespace_keys = [:mobile]
```

And....That's it. Now restart the rails server and we are done!

You can find the working application [here on Github](https://github.com/amitpatelx/devise-mobile-auth) and take a look at commits.

P.S. You can use any custom field instead of `mobile`.