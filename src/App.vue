<template>
  <div id="app" class="w-full h-full fixed p-4 outline-none">
    <div class="w-full h-full">
      <header class="h-1/5 g-center m-auto">
        <div class="text-2xl w-full grid-3">
          <div
            @click="p = !p"
            class="text-white p-2 center border-letter rounded-xl border-2 border-black"
          >
            menu
          </div>
          <div
            class="text-white p-2 center border-letter rounded-xl border-2 border-black"
          >
            {{ s }}
          </div>
          <div
            class="text-white p-2 center border-letter rounded-xl border-2 border-black"
          >
            {{
              `${String(t.m).padStart(2, "0")}:${String(t.s).padStart(2, "0")}`
            }}
          </div>
        </div>
      </header>
      <div class="h-4/5 g-center">
        <div
          v-if="ds[0]"
          id="map"
          class="border-4 rounded-2xl"
          :style="`
            display: grid;
            grid-template-columns: repeat(${Math.sqrt(
              ds.length
            )}, minmax(0, 1fr));
          `"
        >
          <div
            v-for="(d, n) in ds"
            :key="d.id"
            :class="`relative w-full h-full`"
            :style="`
              background-image: url(${require(`@/assets/maps/${m}.jpg`)});
              background-repeat: no-repeat;
              background-size: cover;
              border-${
                n == 0
                  ? 'top-left'
                  : n + 1 == Math.sqrt(ds.length)
                  ? 'top-right'
                  : n + 1 == ds.length
                  ? 'bottom-right'
                  : n == ds.length - Math.sqrt(ds.length)
                  ? 'bottom-left'
                  : ''
              }-radius: 0.8rem
            `"
          >
            <div
              @click="slide(d.d, n + 1)"
              :id="`item-${n + 1}`"
              :class="`absolute w-6/12 h-2/4 top-0 bottom-0 right-0 left-0 m-auto cursor-pointer 
              text-${
                ds == 9 ? '5' : '3'
              }xl duration-700 bg-contain bg-no-repeat bg-center z-50`"
              :style="`background-image: url(${require(`@/assets/cars/${c}/${d.d}.png`)});`"
            ></div>
          </div>
        </div>
      </div>
    </div>
    <menu v-if="p" class="alert-back text-center">
      <div class="alert-front">
        <header class="between">
          <div @click="p = true" :class="`mx-4 ${!p.t ? 'opacity-0' : ''}`">
            &lt;
          </div>
          <h1 class="center">{{ p.t ? p.t : "menu" }}</h1>
          <div class="end mx-4">
            <h1 v-if="ds.filter((i) => i.s == true).length" @click="p = false">
              x
            </h1>
          </div>
        </header>
        <nav v-if="p.n">
          <h2
            @click="
              save('l', 9);
              random(9);
              p = false;
            "
          >
            very easy
          </h2>
          <h2
            @click="
              save('l', 16);
              random(16);
              p = false;
            "
          >
            easy
          </h2>
          <h2
            @click="
              save('l', 25);
              random(25);
              p = false;
            "
          >
            medium
          </h2>
          <h2
            @click="
              save('l', 36);
              random(36);
              p = false;
            "
          >
            hard
          </h2>
          <h2
            @click="
              save('l', 49);
              random(49);
              p = false;
            "
          >
            very hard
          </h2>
        </nav>
        <nav v-else-if="p.o">
          <h1>maps</h1>
          <ul>
            <li
              v-for="ms in ['standard', 'city', 'highway']"
              :key="ms.id"
              @click="
                save('m', ms);
                m = ms;
              "
              :class="`w-12 h-12 rounded-lg ${
                m == ms ? 'border-2 border-blue-400' : ''
              }`"
              :style="`
              background-image: url(${require(`@/assets/maps/${ms}.jpg`)});
              background-repeat: no-repeat;
              background-size: cover;`"
            ></li>
          </ul>
          <h1>cars</h1>
          <ul>
            <li
              v-for="cs in ['police', 'taxi', 'super']"
              :key="cs.id"
              @click="
                save('c', cs);
                c = cs;
              "
              :class="`w-12 h-12 rounded-lg ${
                c == cs ? 'border-2 border-blue-400' : ''
              }`"
              :style="`
              background-image: url(${require(`@/assets/cars/${cs}/↑.png`)});
              background-repeat: no-repeat;
              background-size: cover;`"
            ></li>
          </ul>
          <h1>effects</h1>
          <ul>
            <li
              @click="
                save('v', !e.v);
                e.v = !e.v;
              "
            >
              vibrate: {{ e.v ? "ON" : "Off" }}
            </li>
            <li
              @click="
                save('s', !e.s);
                e.s = !e.s;
              "
            >
              sound: {{ e.s ? "ON" : "Off" }}
            </li>
          </ul>
        </nav>
        <nav v-else>
          <h2 v-if="ds.filter((i) => i.s == true).length" @click="p = false">
            continue
          </h2>
          <h2 @click="p = { t: 'New Game', n: true }">New Game</h2>
          <h2 @click="p = { t: 'option', o: true }">option</h2>
          <h2 v-if="install" @click="install.prompt()">install game</h2>
        </nav>
        <a
          href="https://facebook.com/lazirijs"
          target="_blank"
          class="text-center text-xs text-slate-500"
        >
          created by LAZIRI
        </a>
      </div>
    </menu>
    <div v-else-if="a" class="alert-back">
      <div class="alert-front">
        <h1 class="text-center">{{ a.t ? a.t : "Welcome !" }}</h1>
        <div class="p-8 g-center">
          <svg
            v-if="a.s"
            class="w-12"
            fill="currentColor"
            viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill-rule="evenodd"
              d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
              clip-rule="evenodd"
            ></path>
          </svg>
          <div
            v-else
            class="w-40 h-40 rounded-lg m-auto"
            :style="`
            background-image: url(${require(`@/assets/logo.png`)});
            background-repeat: no-repeat;
            background-size: cover;`"
          ></div>
        </div>
        <h2 class="w-8/12 m-auto text-center">
          {{ a.d ? a.d : "we need your help to solve the Serckala" }}
        </h2>
        <h4 class="text-sub-1">
          {{
            a.sd
              ? a.sd
              : "level " +
                (ds.length == 9
                  ? "very easy"
                  : ds.length == 16
                  ? "easy"
                  : ds.length == 25
                  ? "medium"
                  : ds.length == 36
                  ? "hard"
                  : "very hard")
          }}
        </h4>
        <div dir="ltr" class="between pt-4">
          <button
            v-if="a.n"
            @click="
              p = { t: 'New Game', n: true };
              a = false;
            "
            class="btn w-5/12 bg-blue-400 tracking-wide font-medium m-auto"
          >
            <div class="text-white w-full text-center">new game</div>
          </button>
          <button
            v-else
            @click="a = false"
            class="btn w-5/12 bg-blue-400 tracking-wide font-medium m-auto"
          >
            <div class="text-white w-full text-center">start</div>
          </button>
          <button
            @click="
              p = true;
              a = false;
            "
            class="btn w-5/12 bg-blue-400 tracking-wide font-medium m-auto"
          >
            <div class="text-white w-full text-center">Menu</div>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ds: [],
      c: localStorage.c ? localStorage.c : "police",
      m: localStorage.m ? localStorage.m : "standard",
      s: 0,
      t: {
        m: 0,
        s: 0,
      },
      e: {
        v: localStorage.v !== "false",
        s: localStorage.s !== "false",
      },
      p: false,
      a: true,
      install: null,
    };
  },
  created() {
    this.random(localStorage.l ? Number(localStorage.l) : 9);
    const resize = () => {
      let header = document.querySelector("header");
      let map = document.getElementById("map");

      const h = map.parentElement.offsetHeight;
      const w = map.parentElement.offsetWidth;
      const v = w >= h ? h : w;

      header.style.width = v + "px";
      map.style.height = v + "px";
      map.style.width = v + "px";
    };
    setTimeout(() => {
      resize();
      window.onresize = resize;
    }, 250);
    window.addEventListener("beforeinstallprompt", (e) => {
      e.preventDefault();
      this.install = e;
    });
    setInterval(() => {
      if (!this.p && !this.a) {
        if (this.t.s == 59) {
          this.t.s = 0;
          this.t.m++;
        } else {
          this.t.s++;
        }
      }
    }, 1000);
  },
  methods: {
    random(x = 9) {
      const r = Math.sqrt(x);
      if (Math.floor(r) == r) {
        this.s = 0;
        if (this.ds) {
          for (let i = 0; i < this.ds.length; i++) {
            document.getElementById(`item-${i + 1}`).style.transform = "";
          }
        }
        let f = [];
        for (let i = 0; i < x; i++) {
          let ds = ["←", "→", "↓", "↑"];

          // ↓
          // ↑

          let up = [];

          for (let j = i + 1 - r; j > 0; j -= r) {
            up.push(f[j - 1]);
          }

          if (up.map((arr) => arr.d).includes("↓")) {
            ds = ds.filter((i) => {
              this.t = { m: 0, s: 0 };
              return i != "↑";
            });
          }

          // console.log(up);

          // → ←

          let left = [];

          for (let j = i; j % r != 0; j--) {
            left.push(f[j - 1]);

            for (let y = 1; f[i - y * r]; y++) {
              const ul = {
                i: i + 1,
                r: f[i - y * r].d,
                j_: f[i - left.length].d,
                r_: f[i - y * r - left.length].d,
              };

              // → ↓
              // ↑ ←

              // ↓ ←
              // → ↑

              if (ul.j_ == "↑" && ul.r_ == "→" && ul.r == "↓") {
                ds = ds.filter((i) => {
                  return i != "←";
                });
              } else if (ul.j_ == "→" && ul.r_ == "↓" && ul.r == "←") {
                ds = ds.filter((i) => {
                  return i != "↑";
                });
              }
            }
          }

          if (left.map((arr) => arr.d).includes("→")) {
            ds = ds.filter((i) => {
              return i != "←";
            });
          }

          // console.log(left);

          // console.log(ds);

          const d = ds[Math.floor(Math.random() * ds.length)];

          f.push({ d: d, s: true });

          if (i == x - 1) {
            this.ds = f;
          }
        }
      } else {
        alert(`root of ${x} is not a natural number`);
      }
    },
    slide(d, n) {
      const l = this.ds.length;
      const r = Math.sqrt(l);
      const item = document.getElementById(`item-${n}`);
      const move = () => {
        const dir =
          d == "←" || d == "→"
            ? `X(${d == "→" ? "" : "-"}`
            : `Y(${d == "↓" ? "" : "-"}`;
        item.style.transform = `translate${dir}75rem)`;
        this.ds[n - 1].s = false;
        this.s += 5;
        if (!this.ds.filter((i) => i.s == true).length) {
          const audio = new Audio(require("@/assets/sounds/win.mp3"));
          audio.volume = this.e.s ? 1 : 0;
          audio.play().then(() => {
            this.a = {
              t: "Congratulation !",
              s: true,
              d: `You solved the Serckala in ${
                this.t.m
                  ? this.t.m +
                    ` minute${this.t.m > 1 ? "s" : ""} and ` +
                    this.t.s +
                    ` second${this.t.s > 1 ? "s" : ""}`
                  : this.t.s + ` second${this.t.s > 1 ? "s" : ""}`
              }`,
              sd: "your score is " + this.s,
              n: true,
            };
          });
        }
      };
      const shake = () => {
        const audio = new Audio(require("@/assets/sounds/horn.wav"));
        audio.volume = this.e.s ? 1 : 0;
        audio.play().then(() => {
          window.navigator.vibrate(this.e.v ? 200 : 0);
          item.classList.add("shake");
          this.s -= 10;
          setTimeout(() => {
            item.classList.remove("shake");
          }, 500);
        });
      };

      // ↑↑↑↑↑
      const up = () => {
        let up = [];
        for (let i = n - r; i > 0; i -= r) {
          if (this.ds[i - 1].s) {
            up.push(i);
          }
        }

        return !up.length && d == "↑" ? true : false;
      };

      // ↓↓↓↓↓
      const down = () => {
        let down = [];
        for (let i = n + r; i <= l; i += r) {
          if (this.ds[i - 1].s) {
            down.push(i);
          }
        }

        return !down.length && d == "↓" ? true : false;
      };

      // →→→→→
      const right = () => {
        let right = [];
        for (let i = n + 1; i <= l; i++) {
          if (n % r == 0) {
            break;
          }
          if (this.ds[i - 1].s) {
            right.push(i);
          }
          if (i % r == 0) {
            break;
          }
        }

        return !right.length && d == "→" ? true : false;
      };

      // ←←←←←
      const left = () => {
        let left = [];
        for (let i = n - 1; i % r != 0; i--) {
          if (this.ds[i - 1].s) {
            left.push(i);
          }
        }

        return !left.length && d == "←" ? true : false;
      };

      up() || down() || right() || left() ? move() : shake();
    },
    save(e, d) {
      localStorage.setItem(e, d);
    },
  },
};
</script>

<style>
li {
  margin: auto;
}

ul {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 0.75rem;
  line-height: 1rem;
  margin-bottom: 0.5rem;
}

nav {
  display: grid;
  grid-template-columns: repeat(1, minmax(0, 1fr));
  gap: 1rem;
  margin-top: 1.5rem;
  margin-bottom: 1.5rem;
}

.border-letter {
  -webkit-text-stroke: 1.75px #000;
  background: #7a8a8e;
}

.shake {
  animation: shake 0.5s;
  animation-iteration-count: infinite;
}

@keyframes shake {
  0% {
    transform: translate(1px, 1px) rotate(0deg);
  }
  10% {
    transform: translate(-1px, -2px) rotate(-1deg);
  }
  20% {
    transform: translate(-3px, 0px) rotate(1deg);
  }
  30% {
    transform: translate(3px, 2px) rotate(0deg);
  }
  40% {
    transform: translate(1px, -1px) rotate(1deg);
  }
  50% {
    transform: translate(-1px, 2px) rotate(-1deg);
  }
  60% {
    transform: translate(-3px, 1px) rotate(0deg);
  }
  70% {
    transform: translate(3px, 1px) rotate(-1deg);
  }
  80% {
    transform: translate(-1px, -1px) rotate(1deg);
  }
  90% {
    transform: translate(1px, 2px) rotate(0deg);
  }
  100% {
    transform: translate(1px, -2px) rotate(-1deg);
  }
}
</style>
