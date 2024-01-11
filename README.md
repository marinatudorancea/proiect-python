# proiect-python
# Adoptii-animale
Tip de proiect : Am realizat un site despre adoptia animalelor. 

 

 

Rulare proiect: 

Pentru a putea rula acest proiect este nevoie de: 

 

Descarcarea aplicatiei Vs Code, in care instalam extensia Python;  

Scriem in TERMINAL python.exe -v pip install, urmand sa instalam Flask si Flask-SQLAlchemy cu ajutorul comenzilor pip install Flask si pip install Flask-SQLAlchemy. 

Dupa rulam codul folosind python app.py in command window. 

Dupa efectuarea acestor comenzi o sa se afiseze un link, acesta fiind site ul creat de noi.  

Se copiaza si se insereaza in browser. 

 

Continutul proiectului: 

 

 In fisierul static\uploads am inserat pozele pentru animalele puse spre adoptie regasite pe site. 

In fisierul  templates se regaseste interfata site ului,realizata in html. 

In fisierul app.py am realizat codul in python. 

 

 

Modul de functionare a codului in python: 

 

In fisierul app.py: 

from flask import Flask, request, redirect, url_for, render_template 

from database import db 

from werkzeug.utils import secure_filename 

import os 

 

from models import Animal 

 

Flask: Framework-ul pentru dezvoltarea aplicațiilor web în Python. 

request: Modul pentru gestionarea cererilor HTTP. 

redirect: Funcția pentru redirecționarea către alte rute. 

render_template: Funcția pentru randarea șabloanelor HTML. 

db: Obiectul de bază de date SQLAlchemy. 

secure_filename: Funcția pentru asigurarea unui nume de fișier sigur în timpul încărcării. 

os: Modul pentru interacțiuni cu sistemul de operare. 

Animal: Clasa pentru modelul de date al animalelor. 

 

 

 

 

 

 

2. Configurare aplicație Flask și bază de date: 

app = Flask(__name__) 

#configurare baza de date 

app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///site.db'  # Path pentru db 

db.init_app(app) 

app: Declararea și inițializarea aplicației Flask. 

SQLALCHEMY_DATABASE_URI: Setarea URI-ului pentru baza de date SQLite. 

db.init_app(app): Inițializarea extensiei SQLAlchemy pentru gestionarea bazei de date. 

 

 

3. Configurare pentru încărcarea de fișiere: 

#configurare upload de fisiere 

app.config['UPLOAD_FOLDER'] = 'static/uploads' 

app.config['MAX_CONTENT_LENGTH'] = 16 * 1024 * 1024 

 

ALLOWED_EXTENSIONS = {'png', 'jpg', 'jpeg', 'gif'} 

UPLOAD_FOLDER: Directorul în care vor fi salvate imaginile încărcate. 

MAX_CONTENT_LENGTH: Dimensiunea maximă a conținutului pentru fișierele încărcate (în bytes). 

ALLOWED_EXTENSIONS: Extensiile de fișiere permise pentru încărcare. 

 

4. Funcție pentru verificarea extensiilor de fișiere permise: 

def allowed_file(filename): 

    return '.' in filename and \ 

           filename.rsplit('.', 1)[1].lower() in ALLOWED_EXTENSIONS 

Această funcție verifică dacă extensia unui fișier este printre cele permise. 

 

5. Rută pentru pagina principală 

@app.route('/') 

def home(): 

    animals = Animal.query.all() 

    return render_template('index.html', animals=animals) 

Această parte a codului este responsabilă pentru gestionarea cererilor către pagina principală a site-ului ('/'). Ea extrage toate informațiile despre animalele din baza de date și le afișează pe pagină folosind un șablon HTML numit ‘index.html’. Astfel, atunci când utilizatorii accesează pagina principală, vor vedea o listă cu detalii despre toate animalele disponibile pentru adopție. 

 

 

 

 

 

 

 

 

 

 

6. Rută pentru adăugarea unui animal: 

@app.route('/adauga', methods=['GET', 'POST']) 

def create_animal(): 

    if request.method == 'POST': 

        name = request.form['name'] 

        species = request.form['species'] 

        age = request.form['age'] 

        description = request.form['description'] 

        file = request.files['file'] 

 

        if file and allowed_file(file.filename): 

            filename = secure_filename(file.filename) 

            file_path = os.path.join(app.config['UPLOAD_FOLDER'], filename) 

            file.save(file_path)            

        else: 

            filename = 'default.jpg' 

         

        #adaugare Animal in baza de date 

        new_animal = Animal(name=name, species=species, age=age, description=description, image_file=filename) 

        db.session.add(new_animal) 

        db.session.commit() 

        return redirect(url_for('home')) 

    

    return render_template('create.html') 

 

Această rută se ocupă atât de afișarea paginii de adăugare a animalelor, cât și de procesarea datelor atunci când un utilizator trimite un formular pentru a adăuga un animal nou în sistem. 

 

7. Inițializarea bazei de date și rularea aplicației: 

if __name__ == '__main__': 

    with app.app_context(): 

        db.create_all() 

    app.run(port=8000, debug=True) 

Inițializarea bazei de date cu db.create_all().Rularea aplicației pe portul 8000 în modul de depanare (‘debug=True’). 

Atunci când este setat la True, apar anumite facilități utile în dezvoltarea și testarea aplicației. De exemplu, în cazul unei erori, vom primi informații mai detaliate despre eroare direct în browser. De asemenea, aplicația se va reîncărca automat atunci când detectează modificări în cod. 

 

 

 

In fisierul database.py: 

from flask_sqlalchemy import SQLAlchemy 

 

db = SQLAlchemy() 

Această parte de cod este utilizată pentru a crea o instanță a obiectului SQLAlchemy în cadrul unei aplicații Flask pentru gestionarea bazei de date. 

In fisierul models.py 

from database import db 

 

class Animal(db.Model): 

    id = db.Column(db.Integer, primary_key=True) 

    name = db.Column(db.String(100), nullable=False) 

    species = db.Column(db.String(100), nullable=False) 

    age = db.Column(db.Integer, nullable=False) 

    description = db.Column(db.Text, nullable=False) 

    image_file = db.Column(db.String(20), nullable=False, default='default.jpg') 

 

    def __repr__(self): 

        return f"Animal('{self.name}', '{self.species}', {self.age})" 

 

Acest fragment de cod definesc un model de date Animal care poate fi utilizat pentru a interacționa cu baza de date și a stoca informații despre animale în cadrul aplicației Flask. 

 

Repartizarea sarcinilor de lucru: 

Nuță Andreea:s-a ocupat cu functionalitatea codului, editare, salvare 

Tudorancea Marina s-a ocupat de design ul interfetei site-ului(culori, font uri si aranjare etc.) si de cautarea tutorialelor pentru documentatie. 

 

 

 

Referintele folosite sunt :  

https://youtu.be/dam0GPOAvVI?si=oh9r8CTTS0MSSNjj 

https://www.w3schools.com/python/default.asp 

Conversatia cu ChatGPT este atasata in format txt in folderul B03 pe teams 

