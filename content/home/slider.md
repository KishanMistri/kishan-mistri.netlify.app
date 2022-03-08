---
widget: slider  # Use the Slider widget as this page section
weight: 10  # Position of this section on the page
active: true  # Publish this section?
headless: true  # This file represents a page section.

design:
  # Slide height is automatic unless you force a specific height (e.g. '400px')
  slide_height: '250px'
  is_fullscreen: true
  # Automatically transition through slides?
  loop: true
  # Duration of transition between slides (in ms)
  interval: 2000

content:
  slides:
    - title: 👋 Hey. This site is under maintainance.!
      content: Take a look at what I am working on...
      align: center
      background:
        position: right
        color: '#666'
        brightness: 0.7
        media: slider-frame1.jpg
    - title: Lunch & Learn ☕️
      content: 'Share your knowledge with the group and explore exciting new topics together!'
      align: left
      background:
        position: center
        color: '#555'
        brightness: 0.7
        media: slider-frame2.jpg
    - title: World-Class Algo checking and maintaining
      content: 'Let me know if you have any questions!'
      align: right
      background:
        position: center
        color: '#333'
        brightness: 0.5
        media: slider-frame3.jpg
      link:
        icon: graduation-cap
        icon_pack: fas
        text: Join Us
        url: ../contact/

---
