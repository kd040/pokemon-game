<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pokémon Battle</title>
  <style>
    /* CSS styles */
    .pokemon {
      display: flex;
      justify-content: space-between;
      margin-bottom: 20px;
    }

    .pokemon img {
      max-width: 150px;
      height: auto;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Pokémon Battle</h1>
    <div class="pokemon">
      <div class="pokemon-image">
        <img src="https://assets.pokemon.com/assets/cms2/img/pokedex/full/025.png" alt="Pikachu">
        <p>Pikachu</p>
        <p>HP: <span id="pikachuHP">100</span></p>
      </div>
      <div class="pokemon-image">
        <img src="https://assets.pokemon.com/assets/cms2/img/pokedex/full/004.png" alt="Charmander">
        <p>Charmander</p>
        <p>HP: <span id="charmanderHP">120</span></p>
      </div>
    </div>
    <div class="battle-log" id="battleLog">
      Battle Log:
    </div>
    <div class="actions">
      <button onclick="attack(pikachu, thunderbolt, charmander)">Use Thunderbolt</button>
      <button onclick="attack(pikachu, flamethrower, charmander)">Use Flamethrower</button>
    </div>
  </div>

  <script>
    // JavaScript code
    class Pokemon {
      constructor(name, hp, attack, defense, speed) {
        this.name = name;
        this.hp = hp;
        this.attack = attack;
        this.defense = defense;
        this.speed = speed;
      }

      calculateDamage(move, opponent) {
        const damage = (this.attack / opponent.defense) * move.power;
        return Math.floor(damage);
      }

      attackOpponent(move, opponent) {
        const damage = this.calculateDamage(move, opponent);
        opponent.hp -= damage;
        displayLog(`${this.name} used ${move.name} and dealt ${damage} damage to ${opponent.name}`);
        updateHP();
        if (opponent.hp <= 0) {
          displayLog(`${opponent.name} fainted! ${this.name} wins!`);
          disableMoves();
        }
      }
    }

    class Move {
      constructor(name, type, power) {
        this.name = name;
        this.type = type;
        this.power = power;
      }
    }

    const pikachu = new Pokemon("Pikachu", 100, 50, 40, 60);
    const charmander = new Pokemon("Charmander", 120, 45, 50, 55);

    const thunderbolt = new Move("Thunderbolt", "Electric", 40);
    const flamethrower = new Move("Flamethrower", "Fire", 45);

    const pikachuHP = document.getElementById("pikachuHP");
    const charmanderHP = document.getElementById("charmanderHP");
    const battleLog = document.getElementById("battleLog");

    function displayLog(message) {
      battleLog.innerHTML += `<p>${message}</p>`;
    }

    function updateHP() {
      pikachuHP.textContent = pikachu.hp;
      charmanderHP.textContent = charmander.hp;
    }

    function attack(attacker, move, opponent) {
      attacker.attackOpponent(move, opponent);
    }

    function disableMoves() {
      const buttons = document.querySelectorAll(".actions button");
      buttons.forEach(button => {
        button.disabled = true;
      });
    }

    displayLog("Battle Start!");
    displayLog(`${pikachu.name} vs ${charmander.name}`);
  </script>
</body>
</html>
