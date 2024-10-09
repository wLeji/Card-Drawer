<template>
    <div class="box">
        <div class="card-drawer">
            <h2>Card Drawer</h2>
            <div class="card-container" v-if="!isDrawingSession">
                <div 
                    v-for="tab in tabs" 
                    :key="tab.id" 
                    class="card" 
                    :class="{ sombre: !tab.selected }"
                    @dblclick="toggleOverlay(tab)"
                    @click="selectCard(tab)"
                >
                    <img :src="tab.link" :alt="tab.name" />
                    <p>{{ tab.name }}</p>
                </div>
            </div>

            <button @click="startDrawingSession" v-if="!isDrawingSession">Start Drawing</button>

            <div v-else>
                <div v-if="currentCard">
                    <h3>Drawn Card</h3>
                    <div class="card drawn-card" @click="toggleOverlay(currentCard)">
                        <img :src="currentCard.link" :alt="currentCard.name" />
                        <p>{{ currentCard.name }}</p>
                    </div>
                    <button @click="drawNextCard">Draw Next Card</button>
                </div>
                <div v-else>
                    <h3>No more cards to draw!</h3>
                </div>
            </div>
        </div>

        <!-- Overlay for zoomed card -->
        <div class="overlay" v-if="isOverlayVisible" @click="toggleOverlay(null)">
            <img :src="overlayCard.link" alt="Zoomed Card" class="zoomed-image" />
        </div>
    </div>
</template>



<script>
export default {
    name: 'CardDrawer',
    data() {
        return {
            tabs: [
                { id: 1, name: 'Tab 1', link: 'https://media.wizards.com/2023/one/fr_779a9d966c.png', selected: true },
                { id: 2, name: 'Tab 2', link: 'https://c0.lestechnophiles.com/www.numerama.com/wp-content/uploads/2023/09/champion-magic-erudit-faerie.png?resize=265,370&key=2665460e', selected: true },
                { id: 3, name: 'Tab 3', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 4, name: 'Tab 4', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 5, name: 'Tab 5', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 6, name: 'Tab 6', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 7, name: 'Tab 7', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 8, name: 'Tab 8', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 9, name: 'Tab 9', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 10, name: 'Tab 10', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 11, name: 'Tab 11', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 12, name: 'Tab 12', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 13, name: 'Tab 13', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 14, name: 'Tab 14', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 15, name: 'Tab 15', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 16, name: 'Tab 16', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 17, name: 'Tab 17', link: 'https://picsum.photos/733/1024', selected: true },
                { id: 18, name: 'Tab 18', link: 'https://picsum.photos/733/1024', selected: true }
            ],
            selectedTabs: [],
            isDrawingSession: false,
            deck: [],
            currentCardIndex: 0,
            currentCard: null,
            isOverlayVisible: false,
            overlayCard: null
        };
    },
    methods: {
        selectCard(tab) {
            tab.selected = !tab.selected;
        },
        startDrawingSession() {
            // Filtre les cartes sélectionnées
            this.selectedTabs = this.tabs.filter(tab => tab.selected);
            // Mélange les cartes sélectionnées
            this.deck = this.shuffleArray([...this.selectedTabs]);
            this.currentCardIndex = 0;
            this.isDrawingSession = true; // Démarre la session de dessin
            this.drawNextCard(); // Affiche la première carte
        },
        drawNextCard() {
            if (this.currentCardIndex < this.deck.length) {
                this.currentCard = this.deck[this.currentCardIndex];
                this.currentCardIndex++;
            } else {
                this.currentCard = null; // Aucune carte à tirer
                console.log('No more cards to draw');
            }
        },
        shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        },
        toggleOverlay(card) {
            this.overlayCard = card; // Définit la carte pour l'overlay
            this.isOverlayVisible = !this.isOverlayVisible; // Change l'état de l'overlay
        }
    }
}
</script>



<style scoped>

.box {
    margin: 0 auto;
    padding: 20px;
    background-color: #333;
    max-width: 1200px;
    border: 2px solid #444;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.card-drawer {
    padding: 20px;
}

.card-container {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    justify-content: center;
    margin-top: 30px;
    
}

.card {
    border: 2px solid #ffffff;
    border-radius: 10px;
    padding: 10px;
    width: 150px;
    height: 235px;
    text-align: center;
    transition: opacity 0.3s ease-in-out, background-color 0.3s ease-in-out, color 0.3s ease-in-out, border-color 0.3s ease-in-out;
    background-color: #cbcbcb;
    color: #000;
    opacity: 1;
    cursor: pointer;
}

.drawn-card {
    border: 3px solid #ff0000;
    background-color: #ffcccc;
    color: #ff0000;
    width: 30%;
    height: 300%;
    margin: 0 auto;
}

.card img {
    max-width: 100%;
    height: auto;
    border-radius: 5px;
}

.card.sombre {
    background-color: #333;
    color: #fff;
    border-color: #444;
    opacity: 0.4;
}

button {
    background-color: #007bff;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out;
    display: block;
    margin-left: auto;
    margin-right: auto;
    margin-top: 50px;
}

button:hover {
    background-color: #0056b3;
}

.overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
}

.zoomed-image {
    width: 90vw;
    height: 90vh;
    object-fit: contain;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

</style>
