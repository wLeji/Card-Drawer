<template>
    <div class="box">
        <div class="card-drawer">
            <div id="title">
                <h2>Card Drawer</h2>
            </div>
            <div class="buttons-select" v-if="!isDrawingSession">
                <button @click="filteredTabs.forEach(tab => tab.selected = true)">Select All</button>
                <button @click="filteredTabs.forEach(tab => tab.selected = false)">Unselect All</button>
                <div>
                    <label>
                        <input type="checkbox" v-model="showDCIPromos" @change="unselect_DCI" />
                        DCI Promos
                    </label>
                    <label>
                        <input type="checkbox" v-model="showNicolBolas" @change="unselect_NicolBolas" />
                        Nicol Bolas
                    </label>
                    <label>
                        <input type="checkbox" v-model="showDuskmourn" @change="unselect_Duskmourn" />
                        Duskmourn
                    </label>
                    <h3>selected: {{ 
                        tabs.filter(tab => tab.selected).length 
                    }}</h3>
                </div>
            </div>
            <div class="card-container" v-if="!isDrawingSession">
                <div
                    v-for="tab in filteredTabs" 
                    :key="tab.id"
                    class="card" 
                    :class="{ sombre: !tab.selected }"
                    @dblclick="toggleOverlay(tab)"
                    @click="selectCard(tab)"
                >
                    <img :src="tab.link" :alt="tab.name" />
                    <div id="name-of-cards">
                        <p>{{ tab.name }}</p>
                    </div>
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
                    <div class="buttons-drawings">
                        <button @click="drawNextCard">Draw Next Card</button>
                        <button @click="isDrawingSession = false">Stop Drawing</button>
                    </div>
                    <div class="history">
                    <h3>Drawn Cards History</h3>
                    <div class="card-container" v-if="drawnCards.length > 1">
                        <div 
                            v-for="tab in drawnCards.slice(0, -1).reverse()" 
                            :key="tab.id" 
                            class="card" 
                            @click="toggleOverlay(tab)"
                        >
                            <img :src="tab.link" :alt="tab.name" />
                            <p>{{ tab.name }}</p>
                        </div>
                    </div>

                </div>
                </div>
                <div v-else>
                    <div class="buttons-drawings">
                        <button @click="isDrawingSession = false">Return to menu</button>
                    </div>
                    <div class="history">
                    <h3>Drawn Cards History</h3>
                    <div class="card-container" v-if="drawnCards.length > 1">
                        <div 
                            v-for="tab in drawnCards.reverse()" 
                            :key="tab.id" 
                            class="card" 
                            @click="toggleOverlay(tab)"
                        >
                            <img :src="tab.link" :alt="tab.name" />
                            <p>{{ tab.name }}</p>
                        </div>
                    </div>

                </div>
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
import cardsData from '../../cards.json';
export default {
    name: 'CardDrawer',
    data() {
        return {
            tabs: cardsData.tabs,
            selectedTabs: [],
            isDrawingSession: false,
            deck: [],
            currentCardIndex: 0,
            currentCard: null,
            isOverlayVisible: false,
            overlayCard: null,
            drawnCards: [],
            showDCIPromos: true,
            showNicolBolas: true,
            showDuskmourn: true,
        };
    },
    computed: {
        filteredTabs() {
            return this.tabs.filter(tab => {
                if (tab.extention === "DCI Promos" && !this.showDCIPromos) return false;
                if (tab.extention === "Nicol Bolas" && !this.showNicolBolas) return false;
                if (tab.extention === "Duskmourn" && !this.showDuskmourn) return false;
                return true;
            });
        }
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
            this.drawnCards = [];
            this.isDrawingSession = true; // Démarre la session de dessin
            this.drawNextCard(); // Affiche la première carte
        },
        drawNextCard() {
            if (this.currentCardIndex < this.deck.length) {
                this.currentCard = this.deck[this.currentCardIndex];
                this.drawnCards.push(this.currentCard);
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
        },
        unselect_DCI() {
            this.tabs.forEach(tab => {
                if (tab.extention === "DCI Promos") {
                    tab.selected = false;
                }
            });
        },
        unselect_NicolBolas() {
            this.tabs.forEach(tab => {
                if (tab.extention === "Nicol Bolas") {
                    tab.selected = false;
                }
            });
        },
        unselect_Duskmourn() {
            this.tabs.forEach(tab => {
                if (tab.extention === "Duskmourn") {
                    tab.selected = false;
                }
            });
        }
    }
}
</script>



<style scoped>

#title {
    text-align: center;
    font-size: 2em;
    color: #ffffff;
    margin-bottom: 20px;
}

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
    height: 245px;
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
    width: flex;
    height: 90vh;
    object-fit: contain;
    border: transparent;
    border-radius: 36px;
    cursor: pointer;
}

.buttons-select {
    display: flex;
    justify-content: center;
    gap: 20px;
    margin-bottom: 20px;   
}

.buttons-drawings {
    justify-content: center;
    gap: 20px;
    margin-top: 20px;
    
}

#name-of-cards {
    font-size: 0.8em;
    font-weight: bold;
    
}

</style>
