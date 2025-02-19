import React, { useState, useEffect } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { motion } from "framer-motion";
import { v4 as uuidv4 } from "uuid";

const ADMIN_EMAIL = "owner@example.com";

const pageVariants = {
  initial: { opacity: 0, y: -20 },
  animate: { opacity: 1, y: 0, transition: { duration: 0.8 } },
};

const HomePage = () => {
  const [userEmail, setUserEmail] = useState("guest@example.com");
  const [products, setProducts] = useState([]);
  const [cart, setCart] = useState([]);

  useEffect(() => {
    const storedProducts = JSON.parse(localStorage.getItem("products")) || [
      { id: 1, category: "T-Shirts", image: "/path-to-tshirt1.jpg", name: "Graphic Tee", price: "400 EGP" },
      { id: 3, category: "Hoodies", image: "/path-to-hoodie1.jpg", name: "Oversized Hoodie", price: "800 EGP" },
      { id: 5, category: "Lighters", image: "/path-to-lighter1.jpg", name: "Classic Lighter", price: "600 EGP" }
    ];
    setProducts(storedProducts);
    
    const storedCart = JSON.parse(localStorage.getItem("cart")) || [];
    setCart(storedCart);
  }, []);

  useEffect(() => {
    localStorage.setItem("products", JSON.stringify(products));
  }, [products]);

  useEffect(() => {
    localStorage.setItem("cart", JSON.stringify(cart));
  }, [cart]);

  const addProduct = () => {
    if (userEmail === ADMIN_EMAIL) {
      const newProduct = { id: uuidv4(), category: "New", image: "/path-to-new-item.jpg", name: "New Item", price: "500 EGP" };
      setProducts([...products, newProduct]);
    } else {
      alert("You are not authorized to add products.");
    }
  };

  const removeProduct = (id) => {
    if (userEmail === ADMIN_EMAIL) {
      setProducts(products.filter(product => product.id !== id));
    } else {
      alert("You are not authorized to remove products.");
    }
  };

  const addToCart = (product) => {
    setCart([...cart, product]);
  };

  return (
    <motion.div 
      className="min-h-screen bg-gray-900 text-pink-500 p-8 font-sans"
      initial="initial"
      animate="animate"
      variants={pageVariants}
    >
      <header className="flex justify-between items-center py-4 border-b border-yellow-400">
        <motion.h1 
          className="text-6xl font-extrabold text-yellow-400" 
          initial={{ opacity: 0, scale: 0.8 }} 
          animate={{ opacity: 1, scale: 1 }} 
          transition={{ duration: 0.5 }}>
          GeNeRaTioNzZ
        </motion.h1>
      </header>
      <main className="text-center mt-12">
        <motion.h2 
          className="text-6xl font-extrabold mb-6 text-pink-500" 
          initial={{ opacity: 0, y: -20 }} 
          animate={{ opacity: 1, y: 0 }}>
          View Our Latest Collection
        </motion.h2>
      </main>
      <section className="mt-12">
        {["T-Shirts", "Hoodies", "Lighters"].map(category => (
          <motion.div key={category} id={category.toLowerCase()} className="mb-10" variants={pageVariants}>
            <h2 className="text-4xl font-bold text-yellow-400 mb-6">{category}</h2>
            <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
              {products.filter(product => product.category === category).map((product) => (
                <motion.div key={product.id} whileHover={{ scale: 1.05 }}>
                  <Card className="bg-gray-900 border border-yellow-400 shadow-md rounded-lg">
                    <CardContent>
                      <motion.img 
                        src={product.image} 
                        alt={`Image of ${product.name}`} 
                        className="w-full h-64 object-cover border-b border-yellow-400" 
                        initial={{ opacity: 0 }} 
                        animate={{ opacity: 1 }} 
                        transition={{ duration: 0.5 }}
                      />
                      <h3 className="text-lg font-bold mt-2 text-pink-500">{product.name}</h3>
                      <p className="text-blue-400">{product.price}</p>
                      <Button className="mt-2 bg-blue-400 text-black px-4 py-1 rounded hover:bg-yellow-400" onClick={() => addToCart(product)}>Add to Cart</Button>
                      {userEmail === ADMIN_EMAIL && (
                        <Button className="mt-2 bg-red-500 text-white px-4 py-1 rounded hover:bg-red-400" onClick={() => removeProduct(product.id)}>Remove</Button>
                      )}
                    </CardContent>
                  </Card>
                </motion.div>
              ))}
            </div>
          </motion.div>
        ))}
      </section>
    </motion.div>
  );
};

export default HomePage;

