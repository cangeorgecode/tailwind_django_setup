# tailwind_django_setup
I have downloaded templates from UI deck or Graygrids, how do I set it up with a django backend?   

## Download and extract the templates
Install as instructed
```
npm install
npm run start

# To deploy (yea, you can run this even if you are not deploying)
npm run build
```

## Create a new Django project as normal
Make sure node and npm are installed

## Integrate Tailwind CSS with Django
Move the CSS file from to django
```
mv path/to/tailwind/build/* path/to/proj/core/static/
```

## Configure settings.py as per below (reference https://github.com/cangeorgecode/django-template)
```
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    "whitenoise.middleware.WhiteNoiseMiddleware", # Add this line
]

STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'theme/static'),
]
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
```

## Move the Tailwind HTML templates (*.html) to the templates/core directory

## Set your base.html
```
<link href="{% static 'css/style.css' %}" rel="stylesheet">
```

## Run python manage.py collectstatic 

## style.css error
If you encounter an error - in my style.css, there is a line '@charset "UTF-8";' and I am getting an error in the terminal saying 'at-rule or selector expected'

Move the following line to the top of the style.css file
```
@charset "UTF-8";
```
Then rebuild the Tailwind CSS
```
npm run build
python manage.py collectstatic --clear
```

## Install Tailwind CSS for your django project instead of using play CDN
In your proj directory
```
npm install tailwindcss postcss autoprefixer
npx tailwindcss init
```

Copy and combine the previously downloaded postcss.config.js and tailwind.config.js to the newly created one

## Create your own CSS file
```
mkdir src
touch styles.css

# Add the followings to styles.css
@tailwind base;
@tailwind components;
@tailwind utilities;

# Include the following line in package.json
"scripts": {
  "build:css": "npx tailwindcss -i ./src/styles.css -o ./core/static/css/styles.css --minify"
}

# Run the build script to generate your CSS:
npm run build:css
```

In the base.html, add the newly added styles.css
```
# What a sample base.html looks like

{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Your Project</title>
    <link href="{% static 'css/styles.css' %}" rel="stylesheet">
    <link href="{% static 'style.css' %}" rel="stylesheet"> // You may have 2 CSS files, that's ok
    <script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"></script>  
</head>
<body>
    {% block content %}
    {% endblock content %}
</body>
</html>
```
