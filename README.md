# MEN Full-Stack CRUD

> Membuat aplikasi CRUD sederhana menggunakan Mongodb, ExpressJS dan NodeJS.

## Membuat project baru

Pertama kali yang akan kita lakukan adalah menyiapkan sebuah folder project sebagai contoh kita akan membuat folder dengan nama `contactapp`

```console
$ mkdir contactapp
$ cd contactapp
$ npm init
```

Kita lakukan inisialisasi project kita dengan perintah `npm init` dan selesaikan dengan cara `enter` isi sesuai kebutuhan.

## Install Dependencies

Setelah kita membuat sebuah folder project dan melakukan inisialisasi. Maka, kita akan menginstall dependencies yang diperlukan untuk aplikasi CRUD kita. Diantaranya:

- express@4.17.1
- express-ejs-layouts@2.5.0
- ejs@3.1.6
- nodemon@2.0.16
- mongoose@5.12.13
- method-override@3.0.0
- connect-flash@0.1.1
- cookie-parser@1.4.5
- express-validator@6.12.0
- express-session@1.17.2

Kita bisa mengunduh di [npmjs](https://npmjs.com/)

## Setup ExpressJS

Untuk menggunakan ExpressJS di NodeJS, maka kita perlu setup code dibawah ini di file `index.js` :

```js
const express = require('express');
const app = express();
const port = 3000;

app.listen(port, (req, res) => {
    console.log(`Aplikasi telah berjalan di http://localhost:${port}`);
});
```

## Setup Views

### Setup ExpressLayouts

Untuk setup view, jangan lupa untuk menambahkan code dibawah ini di dalam `index.js`:

```js
const expressLayouts = require('express-ejs-layouts');
app.set('view engine', 'ejs');
app.use(expressLayouts);
```

### Buat Folder Layouts

Buatlah folder dengan nama `views/layouts` dan masing-masing buatkanlah folder dengan rincian

1. Buat folder `main.ejs` di dalam folder `layouts`

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css">

    <title>ExpressJS</title>
  </head>
  <body>
    <%- include('nav') %> 
    <%- body %> >
    
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.12.9/dist/umd/popper.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/js/bootstrap.min.js"></script>
  </body>
</html>          
```

2. Buatlah file `nav.ejs` di dalam folder `layouts` dengan code

```js
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container">
        <a class="navbar-brand" href="/">ContactApp</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav ml-auto">
                <li class="nav-item active">
                    <a class="nav-link" href="/">Home</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="/about">About</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="/contact">Contact</a>
                </li>
            </ul>
        </div>
    </div>
</nav>
```

3. Buatlah file `index.ejs` di dalam folder `views`

```js
<section>
    <div class="container">
        <div class="jumbotron">
            <h1 class="display-4">Halo ExpressJS!</h1>
            <hr class="my-4">
            <p>It uses utility classes for typography and spacing to space content out within the larger container.</p>
            <p class="lead">
                <a class="btn btn-primary btn-sm" href="#" role="button">Learn more</a>
            </p>
        </div>
    </div>
</section>
```
4. Setup route `/` di dalam `index.js`

```js
app.get('/', (req,res) => {
    res.render('index', {
        layout: 'layouts/main'
    });
});
```

## Menambahkan Gambar

Sebelum menambahkan gambar, ada beberapa hal yang harus kita setup. Diantaranya:

1. Gunakan middleware untuk bisa mengakses folder `public`. Tambahkan code dibawah ini:

```js
app.use(express.static('public'));
```

2. Buatlah folder `public/images` kemudian masukanlah gambar yang diinginkan
3. Buatlah `route` baru dengan `/about` di dalam file `views/about.ejs`

```js
<section>
    <div class="container mt-5">
        <div class="row justify-content-center">
            <div class="col-md-6 text-center">
                <img src="/images/lerian.jpg" alt="Lerian" class="img-fluid img-thumbnail">
                <h1 class="display-5 mt-3"><b>Lerian Febriana, A.Md.Kom</b></h1>
                <p class="lead">Informatics Engineer</p>
            </div>
        </div>
    </div>
</section>
```

## Setup Database Mongodb

1. Buatlah folder bernama `utils` dengan nama file `db.js`

```js
const mongoose = require('mongoose');
mongoose.connect('mongodb://127.0.0.1:27017/kanglerian',
{
    useNewUrlParser: true,
    useUnifiedTopology: true,
    useCreateIndex: true
});
```

2. Buatlah file `models/contact.js`

```js
const mongoose = require('mongoose');

/* Membuat schema */
const Contact = mongoose.model('Contact', {
    nama: {
        type: String,
        required: true
    },
    nohp: {
        type: String,
        required: true
    },
    email: {
        type: String
    }
});

module.exports = { Contact }
```
3. Require `db` dengan cara menambahkan code dibawah ini di `index.js`

```js
require('./utils/db');
const { Contact } = require('./models/contact');
```

4. Buatlah `route` untuk menampilkan halaman kontak dan lakukan cek

```js
app.get('/contact', async(req,res) => {
    Contact.find().then((contact) => {
        res.send(contact);
});
```

## Halaman Contacts

1. `contact.ejs` di halaman `views`

```js
<section>
    <div class="container">
        <div class="row">
            <div class="col-md-8">
                <h2 class="my-3"><b>Daftar Contact</b></h2>
                <a href="/contact/add" class="btn btn-primary btn-sm mb-3">Tambah data</a>
                <table class="table table-striped">
                    <thead>
                      <tr>
                        <th scope="col">#</th>
                        <th scope="col">Nama</th>
                        <th scope="col">No HP</th>
                        <th scope="col">Aksi</th>
                      </tr>
                    </thead>
                    <tbody>
                        <% let no = 1; %> 
                        <% contacts.forEach( contact => { %> 
                        <tr>
                          <th scope="row"><%= no++ %></th>
                          <td><%= contact.nama %></td>
                          <td><%= contact.nohp %></td>
                          <td>
                              <a href="#" class="btn btn-success badge rounded-pill">detail</a>
                          </td>
                        </tr>
                        <% }) %>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</section>
```

2. Edit route `/contact` di dalam file `index.js`

```js
app.get('/contact', async (req, res) => {
    const contacts = await Contact.find();
    res.render('contact', {
        layout: 'layouts/main',
        contacts
    });
});
```

## Tambah Kontak

1. Buat file baru di `views/tambah.ejs`

```js
<section>
    <div class="container">
        <div class="row">
            <div class="col-md-6">
                <h2 class="my-3"><b>Tambah data kontak</b></h2>
                <form method="POST" action="/contact">
                    <div class="form-group">
                      <label for="nama">Nama</label>
                      <input type="text" class="form-control" id="nama" name="nama" required>
                    </div>
                    <div class="form-group">
                      <label for="email">Email</label>
                      <input type="email" class="form-control" id="email" name="email" required>
                    </div>
                    <div class="form-group">
                      <label for="nohp">No Handphone</label>
                      <input type="text" class="form-control" id="nohp" name="nohp" required>
                    </div>
                    <button type="submit" class="btn btn-primary btn-sm">Simpan</button>
                </form>
            </div>
        </div>
    </div>
</section>
```

2. Tulis code di `index.js`

```js
app.get('/contact/add', (req,res) => {
    res.render('tambah', {
       layout: 'layouts/main'
    });
});
```

3. Tambahkan code di `index.js`

```js
app.use(express.urlencoded({extended:true}));
```

4. Tambahkan code di `index.js`

```js
app.post('/contact', (req, res) => {
    // res.send(req.body);
    Contact.insertMany(req.body, (error, result) => {
        res.redirect('/contact');
    });
});
```

## Halaman Detail Contact

1. Buatlah halaman `views/detail.ejs`

```js
<section>
    <div class="container mt-3">
        <div class="row">
            <div class="col-md-8">
                <h2><b>Detail Contact</b></h2>
                <div class="card" style="width: 18rem;">
                    <div class="card-body">
                      <h5 class="card-title"><%= contact.nama %></h5>
                      <h6 class="card-subtitle mb-2 text-muted"><%= contact.nohp %></h6>
                      <p class="card-text"><%= contact.email %></p>
                      <a href="/contact" class="card-link d-block mt-3">&laquo; Kembali ke daftar contact</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
```

2. Edit code `views/contact.ejs`

```js
<section>
    <div class="container">
        <div class="row">
            <div class="col-md-8">
                <h2 class="my-3"><b>Daftar Contact</b></h2>
                <a href="/contact/add" class="btn btn-primary btn-sm mb-3">Tambah data</a>
                <table class="table table-striped">
                    <thead>
                      <tr>
                        <th scope="col">#</th>
                        <th scope="col">Nama</th>
                        <th scope="col">No HP</th>
                        <th scope="col">Aksi</th>
                      </tr>
                    </thead>
                    <tbody>
                        <% let no = 1; %> 
                        <% contacts.forEach( contact => { %> 
                        <tr>
                          <th scope="row"><%= no++ %></th>
                          <td><%= contact.nama %></td>
                          <td><%= contact.nohp %></td>
                          <td>
                              <a href="/contact/<%= contact._id %>" class="btn btn-success badge rounded-pill">detail</a>
                          </td>
                        </tr>
                        <% }) %>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</section>
```

3. Tambahkan code di `index.js`

```js
app.get('/contact/:id', async (req, res) => {
    const contact = await Contact.findOne({ _id: req.params.id });
    res.render('detail', {
        layout: 'layouts/main',
        contact
    });
});
```

## Hapus Data Contact

1. Edit halaman `views/detail.ejs`

```js
<section>
    <div class="container mt-3">
        <div class="row">
            <div class="col-md-8">
                <h2><b>Detail Contact</b></h2>
                <div class="card" style="width: 18rem;">
                    <div class="card-body">
                      <h5 class="card-title"><%= contact.nama %></h5>
                      <h6 class="card-subtitle mb-2 text-muted"><%= contact.nohp %></h6>
                      <p class="card-text"><%= contact.email %></p>
                      <form action="/contact?_method=DELETE" method="POST">
                          <input type="hidden" name="id" value="<%= contact._id %>">
                          <button type="submit" class="btn btn-danger badge">hapus</button>
                      </form>
                      <a href="/contact" class="card-link d-block mt-3">&laquo; Kembali ke daftar contact</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
```

2. Tambahkan code `index.js`

```js
const methodOverride = require('method-override');
app.use(methodOverride('_method'));
```

3. Tambahkan code `index.js`

```js
app.delete('/contact', (req, res) => {
    Contact.deleteOne({ _id: req.body.id }).then((result) => {
        res.redirect('/contact')
    });
});
```

## Edit Data Contact

1. Edit halaman `views/detail.ejs`

```js
<section>
    <div class="container mt-3">
        <div class="row">
            <div class="col-md-8">
                <h2><b>Detail Contact</b></h2>
                <div class="card" style="width: 18rem;">
                    <div class="card-body">
                      <h5 class="card-title"><%= contact.nama %></h5>
                      <h6 class="card-subtitle mb-2 text-muted"><%= contact.nohp %></h6>
                      <p class="card-text"><%= contact.email %></p>
                      <a href="/contact/edit/<%= contact._id %>" class="btn btn-warning badge">ubah</a>
                      <form action="/contact?_method=DELETE" method="POST" class="d-inline">
                          <input type="hidden" name="id" value="<%= contact._id %>">
                          <button type="submit" class="btn btn-danger badge">hapus</button>
                      </form>
                      <a href="/contact" class="card-link d-block mt-3">&laquo; Kembali ke daftar contact</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
```

2. Buat halaman `views/edit.ejs`

```js
<section>
    <div class="container">
        <div class="row">
            <div class="col-md-6">
                <h2 class="my-3"><b>Edit data kontak</b></h2>
                <form method="POST" action="/contact?_method=PUT">
                    <input type="hidden" name="_id" value="<%= contact._id %>">
                    <div class="form-group">
                      <label for="nama">Nama</label>
                      <input type="text" class="form-control" id="nama" name="nama" value="<%= contact.nama %>" required>
                    </div>
                    <div class="form-group">
                      <label for="email">Email</label>
                      <input type="email" class="form-control" id="email" name="email" value="<%= contact.email %>" required>
                    </div>
                    <div class="form-group">
                      <label for="nohp">No Handphone</label>
                      <input type="text" class="form-control" id="nohp" name="nohp" value="<%= contact.nohp %>" required>
                    </div>
                    <button type="submit" class="btn btn-primary btn-sm">Ubah data</button>
                </form>
            </div>
        </div>
    </div>
</section>
```

3. Tambahkan code di `index.js`

```js
app.get('/contact/edit/:id', async (req, res) => {
    const contact = await Contact.findOne({ _id: req.params.id });
    res.render('edit', {
        layout: 'layouts/main',
        contact
    });
});

app.put('/contact', (req, res) => {
    Contact.updateOne(
        { _id: req.body._id },
        { $set: {
            nama: req.body.nama,
            nohp: req.body.nohp,
            email: req.body.email,
        }}
    ).then((result) => {
        res.redirect('/contact');
    });
});
```

## Data Validasi

1. Tambahkan code di `index.js`

```js
const flash = require('connect-flash');
const session = require('express-session');
const cookieParser = require('cookie-parser');
const { body, validationResult, check } = require('express-validator');

app.use(cookieParser('secret'));
app.use(session({
   cookie: {maxAge: 6000},
   secret: 'secret',
   resave: true,
   saveUninitialized: true
}));
app.use(flash());
```

2. Tambahkan code di `index.js` bagian `post`

```js
app.post('/contact', [
    check('email', 'Email tidak benar!').isEmail(),
    body('email').custom( async (value) => {
        const duplikat = await Contact.findOne( { email: value } );
        if(duplikat){
            throw new Error('Email sudah digunakan!');
        }
        return true
    }),
    check('nohp', 'No HP tidak benar!').isMobilePhone('id-ID'),
    body('nohp').custom( async (value) => {
        const duplikat = await Contact.findOne( { nohp: value } );
        if(duplikat){
            throw new Error('No HP sudah digunakan!');
        }
        return true
    }),
], (req, res) => {
    const errors = validationResult(req);
    if(!errors.isEmpty()){
        res.render('tambah',{
            layout: 'layouts/main',
            errors: errors.array()
        });
    } else {
        Contact.insertMany(req.body, (error, result) => {
            req.flash('msg', 'Data kontak berhasil ditambah');
            res.redirect('/contact');
        });
    }
});
```

3. Tambahkan code di `index.js` bagian `put`

```js
app.put('/contact', [
    check('email', 'Email tidak benar!').isEmail(),
    body('email').custom( async (value, {req}) => {
        const duplikat = await Contact.findOne({ nohp: value });
        if(value !== req.body.oldemail && duplikat){
            throw new Error('No HP sudah digunakan!');
        }
        return true;
    }),
    check('nohp', 'No HP tidak benar!').isMobilePhone('id-ID'),
    body('nohp').custom( async (value, {req}) => {
        const duplikat = await Contact.findOne({ nohp: value });
        if(value !== req.body.oldnohp && duplikat){
            throw new Error('No HP sudah digunakan!');
        }
        return true;
    }),
], (req, res) => {

    const errors = validationResult(req);
    if(!errors.isEmpty()){
        res.render('edit', {
            layout: 'layouts/main',
            errors: errors.array(),
            contact: req.body
        });
    } else {
        Contact.updateOne(
            { _id: req.body._id },
            { $set: {
                nama: req.body.nama,
                nohp: req.body.nohp,
                email: req.body.email,
            }}
        ).then((result) => {
            req.flash('msg', 'Data kontak berhasil diubah');
            res.redirect('/contact');
        });
    }
});
```

4. Tambahkan code di `views/tambah.ejs` dan `views/edit.ejs`

```js
<% if (typeof errors != 'undefined') { %>
    <div class="alert alert-danger" role="alert">
        <ul>
        <% errors.forEach(error => { %>
            <li><%= error.msg %></li>
        <% }) %>
        </ul>
    </div>
<% } %>
```

5. Tambahkan di form `views/edit.ejs`

```js
<section>
    <div class="container">
        <div class="row">
            <div class="col-md-6">
                <h2 class="my-3"><b>Edit data kontak</b></h2>

                <% if (typeof errors != 'undefined') { %>
                    <div class="alert alert-danger" role="alert">
                      <ul>
                        <% errors.forEach(error => { %>
                         <li><%= error.msg %></li>
                        <% }) %>
                      </ul>
                    </div>
                <% } %>

                <form method="POST" action="/contact?_method=PUT">
                    <input type="hidden" name="_id" value="<%= contact._id %>">
                    <div class="form-group">
                      <label for="nama">Nama</label>
                      <input type="text" class="form-control" id="nama" name="nama" value="<%= contact.nama %>" required>
                    </div>
                    <div class="form-group">
                      <label for="email">Email</label>
                      <input type="hidden" name="oldemail" value="<%= contact.oldemail || contact.email %>">
                      <input type="email" class="form-control" id="email" name="email" value="<%= contact.email %>" required>
                    </div>
                    <div class="form-group">
                      <label for="nohp">No Handphone</label>
                      <input type="hidden" name="oldnohp" value="<%= contact.oldnohp || contact.nohp %>">
                      <input type="text" class="form-control" id="nohp" name="nohp" value="<%= contact.nohp %>" required>
                    </div>
                    <button type="submit" class="btn btn-primary btn-sm">Ubah data</button>
                </form>
            </div>
        </div>
    </div>
</section>
```