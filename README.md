# Shopping-cart-system
# PS No. 11 - Shopping Cart System
# Products: Pen Rs.10, Notebook Rs.50, Bag Rs.500
# GST 18%, 10% discount if subtotal > 500

products = {
    "Pen": 10,
    "Notebook": 50,
    "Bag": 500
}

cart = {}

def view_products():
    print("\nAvailable Products:")
    for name, price in products.items():
        print(f"{name} - Rs.{price}")

def add_to_cart(product, qty):
    # Normalize input - Pen, pen, PEN -> Pen
    product = product.capitalize()
    
    if product not in products:
        print(f"Invalid product. Available: {', '.join(products.keys())}")
        return
    
    if qty < 1:
        print("Quantity must be at least 1.")
        return

    if product in cart:
        cart[product] += qty
    else:
        cart[product] = qty
    
    subtotal = sum(products[p]*q for p,q in cart.items())
    print(f"{product} x{qty} added | Cart subtotal: {subtotal}")

def remove_from_cart(product):
    product = product.capitalize()
    if product not in cart:
        print(f"{product} not in cart")
        return
        
    del cart[product]
    
    if cart:
        cart_detail = ", ".join([f"{k} x{v} = {products[k]*v}" for k,v in cart.items()])
        print(f"{product} removed. Cart: {cart_detail}")
    else:
        print(f"{product} removed. Cart is empty")

def checkout():
    if not cart:
        print("Cart is empty")
        return
        
    subtotal = sum(products[p]*q for p,q in cart.items())
    
    discount = 0
    if subtotal > 500:
        discount = subtotal * 0.10
        
    taxable_amount = subtotal - discount
    gst = taxable_amount * 0.18
    grand_total = taxable_amount + gst
    
    print(f"Subtotal: {int(subtotal)} | Discount: {int(discount)} | GST: {int(gst)}")
    print(f"Grand Total: {int(grand_total)}")
    cart.clear()

# Main console loop
def main():
    while True:
        print("\n--- Shopping Cart Menu ---")
        print("1. View Products  2. Add to Cart  3. Remove  4. Checkout  5. Exit")
        choice = input("Enter choice: ")

        if choice == '1':
            view_products()
        elif choice == '2':
            p = input("Product Name: ")
            try:
                q = int(input("Quantity: "))
                add_to_cart(p, q)
            except ValueError:
                print("Quantity must be a number")
        elif choice == '3':
            p = input("Product to remove: ")
            remove_from_cart(p)
        elif choice == '4':
            checkout()
        elif choice == '5':
            break
        else:
            print("Invalid choice")

if __name__ == "__main__":
    main()
