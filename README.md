const express = require('express');
const app = express();

const port=3000;

app.use(express.json());

let products= [
    {id:1, productname: 'Apples', quantity:100, price:1.50},
    {id:2, productname: 'Bananas', quantity:75, price:0.80},
    {id:3, productname: 'Milk', quantity:50, price:3.50},
    {id:4, productname: 'Bread', quantity:80, price:1.80}
];

app.get('/products', function(req, res){
    res.json({
        status: "success",
        data:{products}
        });
});

app.get('/products/:id', (req, res) => {
    const productId = parseInt(req.params.id);
    const product = products.find((products)=> products.id === productId);

    if (product) {
        res.json(product);
    } else {
        res.status(404).json({message:'Product not found'})
    }
});

app.get('/products/:id', function(req,res){
    const productId = parseInt(req.params.id);
    const product = products.find((product)=> product.id === productId);
    if (product){
        res.json(product);
    } else{
        res.status(404).json({message:'Product not found'});
    }
});

//Add new product
app.post('/products', function(req,res){
    const newProduct = req.body;
    //Assign new ID to student
    newProduct.id = products[products.length-1].id + 1;
    //Add new student to array
    products.push(newProduct);
    //Repond with newly created student in JSON format
    res.status(201).json(newProduct);
});

//Update a product
app.put('/products/:id', function(req,res){
    const productId = parseInt(req.params.id);
    const updateProduct = req.body;

    products = products.map(function(product){

        if (product.id === productId) {
            return {...product, ...updateProduct}; 
        }

        return product;
    });

    res.json(updateProduct);
});

//Delete product by ID
app.delete('/products/:id', function(req,res){
    const productId = parseInt(req.params.id);

    products = products.filter(product => product.id !== productId);

    res.json({message: 'Product deleted successfully'});
});

//Start server
app.listen(port,() => {
    console.log(`Server is running at http://localhost:${port}`);
});
