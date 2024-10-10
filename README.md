![Logo](/src/assets/images/logo-archenemy.png)

[![Netlify Status](https://api.netlify.com/api/v1/badges/bf3c1224-8883-441a-bc46-8c9db992bf82/deploy-status)](https://app.netlify.com/sites/archenemy/deploys)

# Card Drawer

This project is not associated with MTG

The objective of this project is to allow the user to be able to select the cards of their choice, then compose a deck, and draw the cards from this deck one by one in random order.


## Gamemode Archenemy

Although the web interface is quite basic and open to modifications, it was configured to play Magic The Gathering, and particularly Archenemy mode.

## Try it !

If you intend to use this interface, nothing could be simpler, it is hosted [here](https://archenemy.netlify.app/)!

You will then see a list of 102 Archenemy Cards found [here](https://scryfall.com/search?q=t%3Ascheme&order=released&dir=asc&as=grid), with some filters, options and search bar at your disposal. You can then select the cards you want to play with, you can also zoom in on a card with a double click. Then once ready, you can click on the “Start Drawing” button at the bottom. The draw begins with your first card, then you can draw more until the deck is empty. A history also allows you to see the cards drawn since the beginning. You still have the possibility to zoom in on the drawn card or on one from the history via a simple click. Once the game or deck is finished, you can leave the drawing session and start again, your selection has been kept!

## Deployment

Have you tested the project and want to modify it as you wish? It's completely done for, so here's how to do it.
Start by cloning this repository

```bash
git clone https://github.com/wLeji/Card-Drawer.git
```

## How to use

You can find the card list in the cards.json file, modify it as you wish. Each element contains a name, a URL for the image, and a set.

To test launching the page locally, you must have node, and run the command :

```bash
npm run dev
```

## Work In Progress

    - Make a better interface/style

## Authors

- [@wwLeji](https://github.com/wwLeji)
