# Use official Python 3.10-slim image
FROM python:3.10-slim

# Environment settings
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Set work directory
WORKDIR /babyshop_app

# Install Python dependencies
COPY requirements.txt .
RUN pip install --upgrade pip && pip install -r requirements.txt

# Copy project files
COPY . ${WORKDIR}

# Expose Django dev server port
EXPOSE 8000

# Run migrations and then start the development server
CMD python3 manage.py migrate && python3 manage.py runserver 0.0.0.0:8000