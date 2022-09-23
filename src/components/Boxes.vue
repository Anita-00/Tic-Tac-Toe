<template>
    <section class="game--container">
        <div v-for="i in 9"  :key="i">
            <div :id="i" class="cell" @click="playGame(i)">
            </div>
        </div> 
    </section>
    <div>
        <div v-if="isWin">
            <h5> Congratulations! {{winningPlayer}} is the winner! </h5>
        </div>
        <h5 v-else-if="isTie"> Player 1 and Player 2 tied! </h5>
        <h5 v-else> {{isPlayer1 ? 'Player 1' : 'Player 2'}}'s turn </h5>
        <div v-if="isWin || isTie" class="text-center">
            <button type="button" class="btn btn-primary" id="button" @click="playAgain()"> Play Again </button>
        </div>
    </div>
    
    
    
</template>
  
  <script>
    import circle from "./circle.png"
    import x from "./x.png"

    export default {
        data () {
            return {
                isPlayer1: true,
                img: "",
                img1: circle,
                img2: x,
                numTurns: 1,
                isWin: false,
                winningPlayer: "",
                isTie: false
            }
            
        },
        methods: {
            setImage() {
                /* adds an images based on if it is player 1 or player 2's turn */
                if (this.isPlayer1) {
                    this.img = this.img1
                } else {
                    this.img = this.img2
                }
            }, 
            checkLine(i, difference, limit) {
                /* checks if it is a win*/
                /* code works to check row, col, and horizontal wins based on difference and limit */

                let pos = i
                let arr = []
                let boxNode
                let src
                let count = 0

                /* checks if current and higher boxes have the current players image */
                while (pos < limit && pos < 10 && count < 3) {
                    boxNode = document.getElementById(pos)
                    src = boxNode.innerHTML

                    /* if there is at least one box without the players image, false will be returned */
                    if (src.includes(this.img)) {
                        arr.push(true)
                    } else {
                        arr.push(false) 
                    }

                    pos += difference
                    count += 1
                }

                pos = i - difference

                /* checks if lower boxes have the current players image */
                while (count < 3 && pos > 0) {
                    boxNode = document.getElementById(pos)
                    src = boxNode.innerHTML

                    if (src.includes(this.img)) {
                        arr.push(true)
                    } else {
                        arr.push(false) 
                    }

                    pos -= difference
                    count += 1
                }

                /* if there is at least one box without the players image, false will be returned */
                if (arr.includes(false)){
                        return false
                } else {
                    return true
                }
            },
            checkDiag(i) {
                /* sets the parameters to check for a diagnol win from each side */
                let rightDiag = [3,5,7]
                let leftDiag = [1,5,9]
                let isRightWin = false
                let isLeftWin = false
                let limit = 10
                let rightLim = 8
                let rightDiff = 2
                let leftDiff = 4

                if (rightDiag.includes(i)) {
                    isRightWin = this.checkLine(i, rightDiff, rightLim)
                }

                if (leftDiag.includes(i)) {
                    isLeftWin = this.checkLine(i, leftDiff, limit)
                }

                return isRightWin || isLeftWin

            },
            checkWin(i) { 
                /* runs through all the win combinations*/
                let rowLimit = i + (i*5)%3 + 1
                let colLimit = i + 7

                let isRowWin = this.checkLine(i, 1, rowLimit)
                let isColWin = this.checkLine(i, 3, colLimit)
                let isDiagWin = this.checkDiag(i)
                if (isRowWin || isColWin || isDiagWin) {
                    return true
                } else {
                    return false
                }
            }, 
            announceWinner() {
                /* displays the winner message */
                this.isWin = true
                if (this.isPlayer1) {
                    this.winningPlayer = "Player 1"
                } else {
                    this.winningPlayer = "Player 2"
                }
            }, showTieMessage() {
                /* displays the tie message */
                this.isTie = true
                let messageNode = document.getElementById("finalMessage")
                let text = document.createElement("p")
                text.innerHTML = "TEST"
                messageNode.appendChild(text)
            },
            addImage(imgId, boxNode) {
                let newImg = document.createElement("img")
                newImg.src = this.img
                newImg.style.width = '100%'
                newImg.id = imgId
                boxNode.appendChild(newImg)
            },
            fillPlayerSign(i) {
                /* adds the players image and checks if it is a win */
                if (this.isWin === false && this.numTurns < 10) {
                    this.setImage()
                    let imgId = 'img'+ i
                    let imgNode = document.getElementById(imgId)
                    let boxNode = document.getElementById(i)
                    let existingImg = boxNode.contains(imgNode)

                    if (existingImg === false) {
                        /* if there is no previous image in the box, add an image */
                        this.addImage(imgId, boxNode)    
                    }
                }
                
            },
            playGame(i){
                if (this.isWin === false && this.numTurns < 10) {
                    this.fillPlayerSign(i)
                    if (this.numTurns > 4) { 
                        /* has to have minimum 4 turns for a win */                           
                        if (this.checkWin(i)) {
                            this.announceWinner()
                        } 
                        if (this.isWin === false && this.numTurns === 9) {
                            this.showTieMessage()
                        }
                    }
                    
                    this.numTurns += 1
                    this.isPlayer1 = !this.isPlayer1
                    } else if (this.numTurns == 10 ) {
                        this.showTieMessage()
                    } 
                },
            playAgain() {
                /* resets the game */
                this.isPlayer1 = true
                this.img = ""
                this.img1 = circle
                this.img2 = x
                this.numTurns = 1
                this.isWin = false
                this.winningPlayer = ""
                this.isTie = false

                let images = document.getElementsByTagName('img')
                let imgLen = images.length
                for (let i = 0; i < imgLen; i++) {
                    images[0].parentNode.removeChild(images[0]);
                }

            }
        }
    }
    </script>
    
  <!-- Add "scoped" attribute to limit CSS to this component only width: 9vh;-->
  <style scoped>

    .game--container {
        display: grid;
        grid-template-columns: repeat(3, auto);
        width: 50%;
        margin-right: auto;
        margin-left: auto;
    }
    .cell {
        aspect-ratio: 1 / 1;
        width: 8vh;
        box-shadow: 0 0 0 1px #333333;
        border: 1px solid #333333;
        cursor: pointer;
    }
    h5 {
        text-align: center;
        padding-top: 5%;
    }
    #button {
        margin-right: 0;

    }

  </style>
  