<template>
  <div class="modal-wrapper" @click="close">
    <div class="modal" @click.stop="">
      <div class="modal-header">
        <slot name="header"><h1>Instrukcja gry</h1></slot>
        <button type="button" class="btn-close" @click="close">x</button>
      </div>
      <div class="modal-body">
        <slot name="body">
          <div class="cel">
            <h2>Cel gry</h2>
            <p>
              Gra może trwać dowolną ilość czasu. Gracze mogą zakończyć rozgrywkę poprzez naciśnięcie przycisku wyjścia oznaczonego ikonką .
              Wygrywa gracz, który w momencie zakończenia rozgrywki posiada największą ilość punktów.
            </p>
          </div>
          <div class="zasady">
            <h2>Jak grać</h2>
            <p>
              Pionki graczy znajdują się na polu Start. Kolejność ruchów graczy jest zawsze taka sama i jest zależna od kolorów pionków.
              Każdy gracz po kolei rzuca obiema kośćmi na raz i automatycznie przesuwa się o wylosowaną ilość oczek (ruch zgodny z kierunkiem wskazówek zegara).
            </p>
            <h4>Pola</h4>
            <p>
              Jeśli gracz stanie na polu:
              <ul class="zasady-pola">
                <li><em>oznaczonym kolorem - </em>musi wykonać działanie, które się na tym polu znajduje, wpisać je w okienko i zatwierdzić swoją odpowiedź.
                Za poprawną odpowiedź gracz otrzymuje ilość punktów widniejących na polu.</li>
                <li><em>z wykrzyknikiem - </em>czeka go wyzwanie. Na planszy wyświetli się liczba i gracz musi wpisać w okienko działanie, którego wynikiem jest owa liczba.
                Działaniem nie może być mnożenie przez 1. Za poprawną odpowiedź gracz otrzymuje 10 punktów.</li>
                <li><em>więzenia, startu lub białego pola - </em>nic się nie dzieje.</li>
                <li><em>z kajdankami - </em>idzie do więzenia, czyli automatycznie cofa się na polę więzienia i dostaje karę pod postacią 30 punktów ujemnych.</li>
              </ul>
              Za niepoprawny wynik gracz nie otrzymuje punktów ujemnych.
            </p>
            <h4>Inne zasady</h4>
            <p>
              Jeśli gracz otrzyma na obu kostkach taką samą liczbę oczek - rzuca kośćmi jeszcze raz. Jednak jeśli ilość oczek na obu kościach jest taka sama trzy razy z rzędu -
              gracz nie wykonuje ostatniego ruchu, idzie automatycznie do więzienia i dostaje karę pod postacią 30 punktów ujemnych.
            </p>
            <p>
              Za przejście przez pole startu gracz otrzymuje 20 punktów.
            </p>
          </div>
        </slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  methods: {
    close() {
      this.$emit('close');
    }
  }
}
</script>

<style>
.modal-wrapper {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 2;
  cursor: pointer;
}

.modal {
  background: #FFFFFF;
  box-shadow: 2px 2px 20px 1px;
  overflow-x: auto;
  display: flex;
  flex-direction: column;
  border-radius: 30px/10px;
  cursor: default;
  max-width: 80vw;
  max-height: 80vh;
}

.btn-close {
  position: absolute;
  top: 0;
  right: 0;
  border: none;
  font-size: 20px;
  padding: 10px;
  cursor: pointer;
  font-weight: bold;
  color: #4AAE9B;
  background: transparent;
}

.modal-header {
  position: relative;
  border-bottom: 1px solid #eeeeee;
  color: #4AAE9B;
  padding: 1.5rem 2rem;
  text-align: center;
}

.modal-body {
  padding: 3rem 4rem;
}

.modal-body h2 {
  padding-top: 3rem;
  padding-bottom: 1rem;
}

.modal-body h4 {
  padding-top: 1.5rem;
  padding-bottom: 0.5rem;
}

.modal-body li {
  margin-left: 2rem;
  padding-top: 0.5rem;
}

.modal-body li:last-child {
  padding-bottom: 0.5rem;
}

.modal-body .zasady p:last-child {
  padding-top: 0.5rem;
}
</style>