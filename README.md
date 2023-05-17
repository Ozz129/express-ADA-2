# express-ADA-2

// importamos el modulo de express
const express = require('express');

// extraemos el json que contiene la informacion de los libros
const books = require('./books.json')

// utilizamos express
const app = express();

// le digo a mi servidor que entienda como funcionan los datos json
app.use(express.json());

// Definimos un puerto
const port = 3000;

// definimos una ruta de ejemplos
app.get('/ruta', (req, res) => {
    res.send('Hola desde mi primer servidor')
})

//**************/

// Definir rutas para manejas los libros
/**
 * 1. Listar todos los libros
 * 2. Listar un libro en especifico
 * 3. Agregar libros
 * 4. Editar un libro
 * 5. Eliminar un libro
 * 
 * express con todas sus fichas callback => es el encargado de resolver la solicitud
 * app.metodo('/nombreRuta', callback)
 */

app.get('/books', (req, res) => {
    res.send({
        success: true,
        content: books
    })
})

app.post('/book', (req, res) => {
    const book = req.body;
    books.push(book);
    res.send({
        success: true,
        content: books
    }) 
})

app.get('/book/:id', (req, res) => {
    /**
     * ---> estructura de params
     * params = {
     *  "nombreParametro": "valorParametro"
     * }
     */

    const id = req.params.id;
    const book = books.filter(book => book.id == id);

    if (books.length < id) {
        res.status(404).send({
            success: false,
            content: 'Not Found'
        })
    } else {
        res.send({
            success: true,
            content: book
        })
    }
})

app.put('/book/:id', (req, res) => {
    const id = req.params.id; // Obtener el ID del parámetro de la URL
    const { name, autor } = req.body; // Obtener los datos de nombre y autor del cuerpo de la solicitud

    // Buscar el libro en el arreglo por su ID
    const libro = books.find(libro => libro.id == id);

    if (libro) {
        libro.name = name; // Actualizar el nombre del libro
        libro.autor = autor; // Actualizar el autor del libro
        res.json(libro); // Enviar el libro actualizado como respuesta
    } else {
        res.status(404).send('Libro no encontrado'); // Enviar respuesta de error si no se encuentra el libro
    }
});

app.delete('/book/:id', (req, res) => {
    const id = req.params.id; // Obtener el ID del parámetro de la URL
  
    // Buscar el índice del libro en el arreglo por su ID
    const index = books.findIndex(book => book.id == id);
  
    if (index !== -1) {
      books.splice(index, 1); // Eliminar el libro del arreglo
        res.send({
            success: true,
            content: books
        })
    } else {
      res.status(404).send('Libro no encontrado'); // Enviar respuesta de error si no se encuentra el libro
    }
  });
// encender el servidor
app.listen(port, () => {
    console.log('Servidor inicializado en ' + port)
})






-----> Esto iria en otro script, recuerden que este es otro archivo que se llama books.json

[
    {
        "id": 1,
        "name": "cien años de solidad",
        "autor": "Gabriel Garcia Marquez"
    },
    {
        "id": 2,
        "name": "1984",
        "autor": " George Orwell"
    },
    {
        "id": 3,
        "name":"Orgullo y prejuicio",
        "autor":"Jane Austen"
    }
]
