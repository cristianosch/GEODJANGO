# GeoDjango Tutorial Project

This repository contains the code based on the official GeoDjango tutorial. The project demonstrates how to build a geographic web application to visualize world boundaries using Django and the GeoDjango module.

## 🎯 Project Goals

This project teaches you how to:

- Set up a spatial database (PostGIS).
- Work with geographic data in Django, including models with geometric fields.
- Import and manipulate geospatial data (such as shapefiles).
- Display and query spatial data using GeoDjango.

---

## 📦 Prerequisites

Ensure the following are installed in your environment:

- **Python 3.8+**
- **Django 4.x** (or higher)
- **PostgreSQL 13+** with **PostGIS** extension
- **GDAL** and **GEOS** (essential libraries for GeoDjango)

To install the system dependencies on Ubuntu, run:

```bash
sudo apt update
sudo apt install python3-pip python3-venv libpq-dev gdal-bin libgdal-dev postgresql postgresql-contrib postgis
```

---

## 🚀 Project Setup

1. **Clone the repository**

   ```bash
   git clone https://github.com/your-username/geodjango-tutorial.git
   cd geodjango-tutorial
   ```

2. **Create and activate a virtual environment**

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

4. **Set up the database**

   Create a PostgreSQL database with PostGIS support:

   ```sql
   CREATE DATABASE geodjango;
   CREATE EXTENSION postgis;
   ```

   Update the `settings.py` file with your database information:

   ```python
   DATABASES = {
       "default": {
           "ENGINE": "django.contrib.gis.db.backends.postgis",
           "NAME": "geodjango",
           "USER": "your-username",
           "PASSWORD": "your-password",
           "HOST": "localhost",
           "PORT": "",
       }
   }
   ```

5. **Apply migrations**

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

6. **Import geographic data**

   Download and extract the world borders shapefile:

   ```bash
   mkdir world/data
   cd world/data
   wget https://web.archive.org/web/20231220150759/https://thematicmapping.org/downloads/TM_WORLD_BORDERS-0.3.zip
   unzip TM_WORLD_BORDERS-0.3.zip
   cd ../..
   ```

   Use the `LayerMapping` utility to import the shapefile into the database:

   ```bash
   python manage.py shell
   ```

   In the interactive shell, run:

   ```python
   from world.models import WorldBorder
   from django.contrib.gis.utils import LayerMapping

   mapping = {
       'name': 'NAME',
       'area': 'AREA',
       'pop2005': 'POP2005',
       'fips': 'FIPS',
       'iso2': 'ISO2',
       'iso3': 'ISO3',
       'un': 'UN',
       'region': 'REGION',
       'subregion': 'SUBREGION',
       'lon': 'LON',
       'lat': 'LAT',
       'mpoly': 'MULTIPOLYGON',
   }
   lm = LayerMapping(WorldBorder, 'world/data/TM_WORLD_BORDERS-0.3.shp', mapping)
   lm.save(strict=True, verbose=True)
   ```

7. **Run the development server**

   ```bash
   python manage.py runserver
   ```

   Access the project in your browser at `http://127.0.0.1:8000`.

---

## 🌍 Features

- Visualize and manipulate geospatial data in the Django Admin.
- Perform spatial queries and generate maps.
- Work with world border data in standard geospatial formats (such as shapefiles).

---

## 🔧 Technologies Used

- **Django** with the **GeoDjango** module
- **PostGIS** for spatial database support
- **GDAL/OGR** for geospatial data manipulation
- **ESRI Shapefiles** as the data source

---

## 📚 References

- [GeoDjango Documentation](https://docs.djangoproject.com/en/stable/ref/contrib/gis/)
- [Original GeoDjango Tutorial](https://docs.djangoproject.com/en/stable/intro/tutorial01/)

---

## 💃 License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.

---

## ⭐ Give a Star!

If you found this project helpful, don’t forget to leave a star on the repository to show your support!