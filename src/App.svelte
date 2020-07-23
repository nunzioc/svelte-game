<script>
let selected = -1;
const BIG = 50;
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
	playerShips: 0,
	neutralShips: 0,
	size: BIG,
	control: 'player',
}, {
	id: 1,
	playerShips: 0,
	neutralShips: 0,
	size: SMALL,
	control: 'neutral',
}, {
	id: 2,
	playerShips: 0,
	neutralShips: 0,
	size: SMALL,
	control: 'neutral',
}];

const points = (index, i) => {
	let positionX = ((index % 20) * 8) + (i * 250);
    let positionY = Math.floor(index / 20) * 12;
	return (102 + positionX) + "," + (100 + positionY) + " " + 
		(100 + positionX) + "," + (107 + positionY) + " " +
		(107 + positionX) + "," + (103 + positionY);
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
        			cx={150 + (i * 250)}
        			cy="150"
        			r={planet.size}
        			fill={planet.control === 'player' ? "green" : "grey"}
					stroke-width="2"
					stroke={selected === planet.id ? "blue" : "black"}
        		/>
        		<!-- <text
        			x={150 + (i * 250)}
        			y="160"
        			font-size="30"
        			text-anchor="middle"
        			fill="white"
        		>
					{planet.playerShips + planet.neutralShips}
				</text> -->
			</g>
			{#each
				{ length: planet.playerShips + planet.neutralShips }
				as spaceship, index
			}
    			<polygon
					points={points(index,i)}
					style={"fill:" + (index < planet.playerShips ? "lime" : "grey")}
					stroke="black"
					stroke-width="1"
				/>
			{/each}
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
