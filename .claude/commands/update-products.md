# Update Products

Modify the STMNT product showcase.

## Task
Update product information in `src/components/Products.astro`.

## What Can Be Changed
- Product name
- Description
- Price
- Image
- Add/remove products
- Product order

## Instructions
1. Read current Products.astro component
2. Locate the products array
3. Update product information:
   - name: "Product Name"
   - description: "Brief description"
   - price: "€XX.00"
   - image: "/path/to/product.jpg"
4. Maintain 3-column grid on desktop
5. Test responsive layout (1 col mobile, 2 col tablet, 3 col desktop)
6. Verify all images load

## Example
```javascript
{
  name: 'Matte Pomade',
  description: 'Strong hold con acabado mate natural',
  price: '€18.00',
  image: '/images/products/pomade.jpg'
}
```
