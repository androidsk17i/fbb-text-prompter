<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Asian Female Bodybuilder Prompt Generator</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap');
        
        body {
            font-family: 'Roboto', sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
        }
        .container {
            max-width: 800px;
            margin: auto;
            background: rgba(255, 255, 255, 0.9);
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }
        h1 {
            color: #4a4a4a;
            text-align: center;
            font-size: 2.5em;
            margin-bottom: 30px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }
        h2 {
            color: #5a67d8;
            margin-top: 20px;
        }
        select, textarea, input[type="text"] {
            width: 100%;
            padding: 12px;
            margin-bottom: 15px;
            border: 2px solid #5a67d8;
            border-radius: 8px;
            font-size: 16px;
            transition: all 0.3s ease;
        }
        select:focus, textarea:focus, input[type="text"]:focus {
            outline: none;
            border-color: #4c51bf;
            box-shadow: 0 0 0 3px rgba(90,103,216,0.3);
        }
        .option-container {
            background: #f0f4ff;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 25px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            transition: transform 0.3s ease;
        }
        .option-container:hover {
            transform: translateY(-5px);
        }
        .option-container select {
            flex: 1;
        }
        .option-container input[type="text"] {
            flex: 2;
            display: none;
        }
        button {
            display: block;
            width: 100%;
            padding: 12px;
            background: #5a67d8;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 18px;
            margin-top: 20px;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        button:hover {
            background: #4c51bf;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
        }
        .output-container {
            display: flex;
            gap: 20px;
            margin-top: 30px;
        }
        #positiveOutput, #negativeOutput {
            flex: 1;
            padding: 20px;
            background: #e6e8ff;
            border-radius: 10px;
            font-size: 16px;
            white-space: pre-wrap;
            word-wrap: break-word;
            border: 2px solid #5a67d8;
        }
        .button-container {
            display: flex;
            justify-content: space-between;
            gap: 20px;
        }
        #copyNotification {
            text-align: center;
            color: #38a169;
            margin-top: 15px;
            font-weight: bold;
            font-size: 18px;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        .fade-in {
            animation: fadeIn 0.5s ease-in;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>✨ Asian Female Bodybuilder Prompt Generator ✨</h1>
        <div id="options"></div>
        <h2>🚫 Negative Words</h2>
        <textarea id="negativeWords" rows="4">(nsfw, naked, nude, deformed iris, deformed pupils, semi-realistic, cgi, 3d, render, sketch, cartoon, drawing, anime, mutated hands and fingers:1.4), (deformed, distorted, disfigured:1.3), poorly drawn, bad anatomy, wrong anatomy, extra limb, missing limb, floating limbs, disconnected limbs, mutation, mutated, ugly, disgusting, amputation</textarea>
        <h2>🚫 SDXL Negative Words</h2>
        <textarea id="sdxlNegativeWords" rows="4">(octane render, render, drawing, anime, bad photo, bad photography:1.3), (worst quality, low quality, blurry:1.2), (bad teeth, deformed teeth, deformed lips), (bad anatomy, bad proportions:1.1), (deformed iris, deformed pupils), (deformed eyes, bad eyes), (deformed face, ugly face, bad face), (deformed hands, bad hands, fused fingers), morbid, mutilated, mutation, disfigured</textarea>
        <div class="button-container">
            <button onclick="generatePrompt()">🎨 Generate Prompt</button>
            <button onclick="generateRandomPrompt()">🎲 Random Prompt</button>
        </div>
        <div class="output-container">
            <div id="positiveOutput"></div>
            <div id="negativeOutput"></div>
        </div>
        <div id="copyNotification"></div>
    </div>

    <script>
        const categories = {
    nations: ['Chinese', 'Japanese', 'Korean', 'Thai', 'Vietnamese', 'Filipino', 'Indonesian', 'Malaysian', 'Singaporean', 'Taiwanese', 'Mongolian', 'Nepalese', 'Bhutanese', 'Cambodian', 'Hong Kong', 'Other'],
    breastsSizes: ['small', 'medium', 'large', 'extra large', 'huge', 'massive', 'gigantic', 'petite', 'modest', 'ample', 'voluptuous', 'buxom', 'busty', 'well-endowed', 'proportionate', 'Other'],
    poses: ['double bicep pose', 'lat spread', 'side chest pose', 'front relaxed stance', 'back double bicep pose', 'most muscular pose', 'abdominal and thigh pose', 'side tricep pose', 'front lat spread', 'rear lat spread', 'vacuum pose', 'archer pose', 'hands on hips pose', 'side serratus pose', 'crab pose', 'Other'],
    clothes: ['revealing string bikini', 'high-cut thong bikini', 'micro bikini', 'fishnet bikini', 'metallic gold bikini', 'sheer mesh bikini', 'strappy cutout bikini', 'neon sports bra and shorts', 'latex bodysuit', 'oil-slicked skin', 'body paint', 'shimmering sequin bikini', 'animal print bikini', 'holographic bikini', 'leather harness', 'Other'],
    scenes: ['luxurious beach resort', 'private yacht deck', 'high-end fitness studio', 'exclusive rooftop pool', 'tropical waterfall', 'sunset-lit balcony', 'opulent hotel suite', 'ancient temple ruins', 'futuristic cityscape', 'misty mountain peak', 'cherry blossom garden', 'neon-lit urban alley', 'serene zen garden', 'bustling night market', 'bamboo forest', 'Other'],
    lightings: ['dramatic side lighting', 'soft diffused light', 'golden hour glow', 'high-contrast studio lights', 'cool blue mood lighting', 'warm sunset hues', 'dramatic backlighting', 'neon color splash', 'ethereal misty glow', 'dramatic chiaroscuro', 'vibrant rainbow lighting', 'moody low-key lighting', 'lens flare highlight', 'bioluminescent glow', 'starry night sky illumination', 'Other'],
    expressions: ['seductive gaze', 'confident smirk', 'alluring smile', 'sultry pout', 'fierce determination', 'playful wink', 'intense eye contact', 'triumphant grin', 'mysterious half-smile', 'focused concentration', 'joyful laughter', 'serene meditation', 'passionate intensity', 'coy side glance', 'powerful roar', 'Other'],
    compositions: ['half body shot', 'full body portrait', 'close-up on upper body', 'wide shot showing full figure', '3/4 view portrait', 'dynamic action shot', 'stylized silhouette', 'bird\'s eye view', 'low angle shot', 'Dutch angle', 'panoramic landscape view', 'symmetrical composition', 'rule of thirds framing', 'leading lines perspective', 'extreme close-up on muscles', 'Other'],
    lenses: ['24mm f/1.4', '35mm f/1.8', '50mm f/1.8', '85mm f/1.4', '100mm f/2.8 macro', '70-200mm f/2.8', '16-35mm f/2.8', '24-70mm f/2.8', '135mm f/2', '200mm f/2', '300mm f/2.8', '400mm f/2.8', '600mm f/4', 'fisheye 8mm f/3.5', 'tilt-shift 90mm f/2.8', 'Other']
        };

        function createSelect(category, options) {
            const container = document.createElement('div');
            container.className = 'option-container fade-in';

            const select = document.createElement('select');
            select.id = category;
            select.onchange = function() { toggleCustomInput(category); };

            const label = document.createElement('h2');
            label.textContent = `🔹 ${category.charAt(0).toUpperCase() + category.slice(1)}`;

            options.forEach(option => {
                const optionElement = document.createElement('option');
                optionElement.value = option;
                optionElement.textContent = option;
                select.appendChild(optionElement);
            });

            const customInput = document.createElement('input');
            customInput.type = 'text';
            customInput.id = `${category}Custom`;
            customInput.placeholder = `Enter custom ${category}`;

            container.appendChild(label);
            container.appendChild(select);
            container.appendChild(customInput);

            return container;
        }

        function toggleCustomInput(category) {
            const select = document.getElementById(category);
            const customInput = document.getElementById(`${category}Custom`);
            if (select.value === 'Other') {
                customInput.style.display = 'block';
            } else {
                customInput.style.display = 'none';
            }
        }

        function initializeOptions() {
            const optionsContainer = document.getElementById('options');
            for (const [category, options] of Object.entries(categories)) {
                optionsContainer.appendChild(createSelect(category, options));
            }
        }

        function generatePrompt() {
            const selections = {};
            for (const category of Object.keys(categories)) {
                const select = document.getElementById(category);
                const customInput = document.getElementById(`${category}Custom`);
                selections[category] = select.value === 'Other' ? customInput.value : select.value;
            }
            setPrompt(selections);
        }

        function generateRandomPrompt() {
            const selections = {};
            for (const [category, options] of Object.entries(categories)) {
                const randomOption = options[Math.floor(Math.random() * (options.length - 1))]; // Exclude 'Other'
                selections[category] = randomOption;
                document.getElementById(category).value = randomOption;
                document.getElementById(`${category}Custom`).style.display = 'none';
            }
            setPrompt(selections);
        }

        function setPrompt(selections) {
            const positivePrompt = `${selections.compositions} of a beautiful chubby muscular ${selections.nations} female bodybuilder with ${selections.breastsSizes} breasts, ${selections.poses}, wearing ${selections.clothes}, in a ${selections.scenes}, with ${selections.lightings}, ${selections.expressions} expression, shot by a ${selections.lenses}, photorealistic, highly detailed, 8k resolution, natural skin textures.`;
            const negativePrompt = document.getElementById('negativeWords').value;
            const sdxlNegativePrompt = document.getElementById('sdxlNegativeWords').value;
            
            const positiveOutputElement = document.getElementById('positiveOutput');
            const negativeOutputElement = document.getElementById('negativeOutput');
            
            positiveOutputElement.textContent = positivePrompt;
            negativeOutputElement.textContent = `Regular Negative:\n${negativePrompt}\n\nSDXL Negative:\n${sdxlNegativePrompt}`;
            
            positiveOutputElement.classList.add('fade-in');
            negativeOutputElement.classList.add('fade-in');
            
            setTimeout(() => {
                positiveOutputElement.classList.remove('fade-in');
                negativeOutputElement.classList.remove('fade-in');
            }, 500);
            
            copyToClipboard(positivePrompt);
        }

        function copyToClipboard(text) {
            navigator.clipboard.writeText(text).then(() => {
                const notification = document.getElementById('copyNotification');
                notification.textContent = '✅ Positive prompt copied to clipboard!';
                notification.classList.add('fade-in');
                setTimeout(() => {
                    notification.classList.remove('fade-in');
                    notification.textContent = '';
                }, 2000);
            }).catch(err => {
                console.error('Failed to copy: ', err);
            });
        }

        initializeOptions();
    </script>
</body>
</html>
