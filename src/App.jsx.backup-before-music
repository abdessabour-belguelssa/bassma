import { useEffect, useRef, useState } from "react";
import gsap from "gsap";
import "./App.css";

function App() {
  const canvasRef = useRef(null);
  const [scene, setScene] = useState("intro");
  const [envelopeOpened, setEnvelopeOpened] = useState(false);

  const memories = [
    { image: "/memories/memory-01-sunset-selfie.jpeg", title: "Our sunset", text: "A moment I want to keep forever." },
    { image: "/memories/memory-02-together.jpeg", title: "Together", text: "My favourite place is beside you." },
    { image: "/memories/memory-03-garden-photo.jpeg", title: "Little moments", text: "Every moment with you means something." },
    { image: "/memories/memory-04-ride-together.jpeg", title: "Our adventures", text: "And so many memories still to come." },
    { image: "/memories/memory-05-funny-collage.jpeg", title: "Just us", text: "My favourite kind of happiness." },
  ];


  // Three.js — universe with thousands of stars
  useEffect(() => {
    let renderer;
    let animationId;
    let geometry;
    let material;
    let stars;
    let active = true;

    const startUniverse = async () => {
      const THREE = await import("three");
      if (!active || !canvasRef.current) return;

      const canvas = canvasRef.current;
      const universe = new THREE.Scene();
      universe.fog = new THREE.FogExp2(0x03030b, 0.035);

      const camera = new THREE.PerspectiveCamera(
        60,
        window.innerWidth / window.innerHeight,
        0.1,
        100
      );
      camera.position.z = 18;

      renderer = new THREE.WebGLRenderer({
        canvas,
        alpha: true,
        antialias: true,
      });

      renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
      renderer.setSize(window.innerWidth, window.innerHeight);

      const starCount = 2500;
      const positions = new Float32Array(starCount * 3);

      for (let i = 0; i < starCount; i++) {
        positions[i * 3] = (Math.random() - 0.5) * 65;
        positions[i * 3 + 1] = (Math.random() - 0.5) * 45;
        positions[i * 3 + 2] = (Math.random() - 0.5) * 45;
      }

      geometry = new THREE.BufferGeometry();
      geometry.setAttribute(
        "position",
        new THREE.BufferAttribute(positions, 3)
      );

      material = new THREE.PointsMaterial({
        color: 0xd8d5ff,
        size: 0.055,
        transparent: true,
        opacity: 0.85,
        sizeAttenuation: true,
      });

      stars = new THREE.Points(geometry, material);
      universe.add(stars);

      const animate = () => {
        animationId = requestAnimationFrame(animate);
        stars.rotation.y += 0.00025;
        stars.rotation.x += 0.00008;
        renderer.render(universe, camera);
      };

      animate();

      const handleResize = () => {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
      };

      window.addEventListener("resize", handleResize);

      renderer.userData = { handleResize };
    };

    startUniverse();

    return () => {
      active = false;
      cancelAnimationFrame(animationId);

      if (renderer) {
        window.removeEventListener("resize", renderer.userData.handleResize);
        renderer.dispose();
      }

      geometry?.dispose();
      material?.dispose();
    };
  }, []);

  // Cinematic transitions between scenes
  useEffect(() => {
    const context = gsap.context(() => {
      if (scene === "intro") {
        gsap.fromTo(
          ".intro-content > *",
          { opacity: 0, y: 25 },
          {
            opacity: 1,
            y: 0,
            duration: 1.1,
            stagger: 0.18,
            ease: "power3.out",
          }
        );
      }

      if (scene === "envelope") {
        gsap.fromTo(
          ".envelope-scene > *",
          { opacity: 0, y: 25 },
          {
            opacity: 1,
            y: 0,
            duration: 1.1,
            stagger: 0.2,
            ease: "power3.out",
          }
        );
      }

      if (scene === "letter") {
        gsap.fromTo(
          ".letter-scene",
          { opacity: 0, scale: 0.96, y: 20 },
          {
            opacity: 1,
            scale: 1,
            y: 0,
            duration: 1.2,
            ease: "power3.out",
          }
        );
      }
    });

    return () => context.revert();
  }, [scene]);

  useEffect(() => {
    if (scene !== "memories") return;

    const context = gsap.context(() => {
      gsap.fromTo(
        ".memory-card",
        { opacity: 0, y: 35 },
        {
          opacity: 1,
          y: 0,
          duration: 0.8,
          stagger: 0.15,
          ease: "power3.out",
        }
      );
    });

    return () => context.revert();
  }, [scene]);

  const enterUniverse = () => {
    setScene("envelope");
  };

  const openEnvelope = () => {
    if (envelopeOpened) return;

    setEnvelopeOpened(true);

    gsap.to(".envelope-flap", {
      rotateX: 180,
      duration: 0.9,
      ease: "power2.inOut",
    });

    gsap.to(".envelope-letter", {
      y: -225,
      duration: 1.5,
      delay: 0.45,
      ease: "power3.out",
      onComplete: () => {
        setTimeout(() => setScene("letter"), 500);
      },
    });
  };

  return (
    <main className="universe">
      <canvas ref={canvasRef} className="star-canvas" />

      <div className="ambient-glow glow-one" />
      <div className="ambient-glow glow-two" />

      {scene === "intro" && (
        <section className="hero-content intro-content">
          <p className="eyebrow">
            <span className="eyebrow-line" />
            A LITTLE UNIVERSE, JUST FOR YOU
            <span className="eyebrow-line" />
          </p>

          <h1 className="hero-title">
            THE ONLY
            <span>STAR</span>
          </h1>

          <p className="hero-description">
            In a universe full of stars,
            <br />
            my eyes will always search for you.
          </p>

          <button className="enter-button" onClick={enterUniverse}>
            <span>ENTER OUR UNIVERSE</span>
            <span className="button-arrow">↗</span>
          </button>
        </section>
      )}

      {scene === "envelope" && (
        <section className="envelope-scene">
          <p className="eyebrow">
            <span className="eyebrow-line" />
            A MESSAGE FROM MY HEART
            <span className="eyebrow-line" />
          </p>

          <h2 className="scene-title">Something for you...</h2>

          <p className="scene-subtitle">
            A little letter, carrying a lot of love.
          </p>

          <button
            className={`envelope-button ${envelopeOpened ? "opened" : ""}`}
            onClick={openEnvelope}
            disabled={envelopeOpened}
            aria-label="Open your love letter"
          >
            <div className="envelope">
              <div className="envelope-letter">
                <span className="letter-mini-heart">♥</span>
                <span className="letter-mini-text">For you</span>
              </div>

              <div className="envelope-back" />
              <div className="envelope-front" />
              <div className="envelope-flap" />

              <div className="envelope-seal">♥</div>
            </div>
          </button>

          <p className="envelope-hint">
            {envelopeOpened ? "OPENING WITH LOVE..." : "TAP THE ENVELOPE TO OPEN"}
          </p>
        </section>
      )}

      {scene === "letter" && (
        <section className="letter-scene">
          <div className="letter-paper">
            <p className="letter-kicker">A LETTER FOR YOU</p>

            <div className="letter-divider">✦</div>

            <h2 className="letter-heading">To my only star,</h2>

            <p className="letter-paragraph">
              If I could give you one thing in this universe,
              it would be the ability to see yourself through my eyes.
            </p>

            <p className="letter-paragraph">
              Then you would understand how special you are,
              how much light you bring into my world,
              and why my heart keeps choosing you.
            </p>

            <p className="letter-paragraph">
              No matter how many stars fill the sky,
              you will always be my favourite one.
            </p>

            <p className="letter-signature">With all my love,</p>
            <p className="letter-name">Yours ♥</p>
          </div>

          <button
            className="memories-button"
            onClick={() => setScene("memories")}
          >
            RELIVE OUR MEMORIES <span>✦</span>
          </button>
          <p className="letter-bottom">OUR STORY IS JUST BEGINNING</p>
        </section>
      )}

      {scene === "memories" && (
        <section className="memories-scene">
          <p className="eyebrow">
            <span className="eyebrow-line" />
            LITTLE MOMENTS, BIG MEMORIES
            <span className="eyebrow-line" />
          </p>
          <h2 className="scene-title">Our little universe</h2>
          <p className="scene-subtitle">
            Every picture holds a piece of my heart.
          </p>

          <div className="memories-grid">
            {memories.map((memory, index) => (
              <article className="memory-card" key={memory.image}>
                <div className="memory-image-wrap">
                  <img
                    src={memory.image}
                    alt={memory.title}
                    loading="lazy"
                  />
                  <span className="memory-number">
                    {String(index + 1).padStart(2, "0")}
                  </span>
                </div>
                <div className="memory-info">
                  <h3>{memory.title}</h3>
                  <p>{memory.text}</p>
                </div>
              </article>
            ))}
          </div>
          <p className="memories-ending">AND THIS IS ONLY THE BEGINNING ♥</p>
        </section>
      )}

      <div className="bottom-caption">
        <span>MADE WITH LOVE</span>
        <span className="caption-star">✦</span>
        <span>FOR ONE SOUL</span>
      </div>

      <div className="corner-label">
        {scene === "intro" ? "01 / 04" : scene === "envelope" ? "02 / 04" : scene === "letter" ? "03 / 04" : "04 / 04"}
      </div>
    </main>
  );
}

export default App;
