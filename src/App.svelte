<script>
let selected = -1;
const BIG = 50;
const MEDIUM = 40;
const SMALL = 30;

let seconds = 0;
setInterval(() => {
	planets = planets.map(planet => {
		// fighting?
		if (planet.playerShips && planet.neutralShips) {
			planet.playerShips--;
			planet.neutralShips--;
			if (planet.neutralShips === 0 && planet.playerShips > 0) {
				planet.control = 'player';
			}
		} else if (planet.size === BIG) {
			if (planet.control === 'player') {
    			planet.playerShips++;
			} else {
				planet.neutralShips++;
			}
		}
		if (seconds % 2 === 0 && planet.size === MEDIUM) {
			if (planet.control === 'player') {
    			planet.playerShips++;
			} else {
				planet.neutralShips++;
			}
		}
		if (seconds % 3 === 0 && planet.size === SMALL) {
			if (planet.control === 'player') {
    			planet.playerShips++;
			} else {
				planet.neutralShips++;
			}
		}
		return planet;
	});	
	seconds++;
}, 1000);

let planets = [{
	id: 0,
	x: 100,
	y: 100,
	playerShips: 0,
	neutralShips: 0,
	size: MEDIUM,
	control: 'player',
}, {
	id: 1,
	x: 350,
	y: 100,
	playerShips: 0,
	neutralShips: 0,
	size: SMALL,
	control: 'neutral',
}, {
	id: 2,
	x: 250,
	y: 250,
	playerShips: 0,
	neutralShips: 0,
	size: SMALL,
	control: 'neutral',
}, {
	id: 3,
	x: 450,
	y: 300,
	playerShips: 0,
	neutralShips: 0,
	size: BIG,
	control: 'neutral',
}];

const lines = [[0,1],[0,2],[2,3]];

const points = (x, y, index) => {
	x = x - 50;
	y = y - 50;
	let positionX = ((index % 15) * 8) ;
    let positionY = Math.floor(index / 15) * 12;
	return (x + 2 + positionX) + "," + (y + positionY) + " " + 
		(x + positionX) + "," + (y + 7 + positionY) + " " +
		(x + 7 + positionX) + "," + (y + 3 + positionY);
}
</script>

<main on:click={() => { selected = -1; }}>
	<svg width="800" height="600" baseProfile="full">
		{#each planets as planet, i (planet.id)}
			<g
    			on:click|stopPropagation={() => {
    				if (selected != -1 && selected !== planet.id) {
    					planet.playerShips += planets[selected].playerShips;
						planets[selected].playerShips = 0;
    					planets = planets;
    				}
    				selected = planet.id;
    			}}
			>
        		<circle
        			cx={planet.x}
        			cy={planet.y}
        			r={planet.size}
        			fill={planet.control === 'player' ? "green" : "grey"}
					stroke-width="2"
					stroke={selected === planet.id ? "blue" : "black"}
        		/>
			</g>
			{#each
				{ length: planet.playerShips + planet.neutralShips }
				as spaceship, index
			}
    			<polygon
					points={points(planet.x, planet.y, index)}
					style={"fill:" + (index < planet.playerShips ? "lime" : "grey")}
					stroke="black"
					stroke-width="1"
				/>
			{/each}
		{/each}
		{#each lines as line}
		{/each}
	</svg>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
