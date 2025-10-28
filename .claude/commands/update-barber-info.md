# Update Barber Info

Modify barber details in the team section.

## Task
Update barber information in `src/components/Team.astro`.

## What Can Be Changed
- Barber name
- Specialty/title
- Photo
- Booksy booking link
- Add/remove barbers

## Instructions
1. Read current Team.astro component
2. Locate the barbers array
3. Update the specific barber's information:
   - name: "Full Name"
   - specialty: "Specialty Description"
   - image: "/path/to/image.jpg"
   - booksy: "https://booksy.com/link"
4. If adding a 5th barber, adjust grid layout (consider 3-2 or 5-column layout)
5. Test responsive layout
6. Verify Booksy links work

## Example
```javascript
{
  name: 'Carlos García',
  specialty: 'Fade Expert & Classic Cuts',
  image: '/images/team/carlos.jpg',
  booksy: 'https://booksy.com/privatestudio/carlos'
}
```
