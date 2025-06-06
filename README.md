<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>随机饮食训练计划生成器</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
        }

        .header p {
            font-size: 1.1em;
            opacity: 0.9;
        }

        .content {
            padding: 30px;
        }

        .settings-panel {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 30px;
            border: 2px solid #e9ecef;
        }

        .settings-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 20px;
        }

        .setting-group {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
        }

        .setting-group h3 {
            color: #333;
            margin-bottom: 15px;
            font-size: 1.1em;
        }

        .form-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
            color: #555;
        }

        input, select {
            width: 100%;
            padding: 10px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 14px;
            transition: border-color 0.3s;
        }

        input:focus, select:focus {
            outline: none;
            border-color: #4facfe;
        }

        .checkbox-group {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
        }

        .checkbox-item {
            display: flex;
            align-items: center;
            background: #f8f9fa;
            padding: 8px 12px;
            border-radius: 20px;
            border: 1px solid #dee2e6;
            cursor: pointer;
            transition: all 0.3s;
        }

        .checkbox-item:hover {
            background: #e9ecef;
        }

        .checkbox-item input[type="checkbox"] {
            width: auto;
            margin-right: 8px;
        }

        .checkbox-item.checked {
            background: #4facfe;
            color: white;
            border-color: #4facfe;
        }

        .generate-btn {
            background: linear-gradient(135deg, #ff6b6b 0%, #ee5a24 100%);
            color: white;
            border: none;
            padding: 15px 40px;
            border-radius: 50px;
            font-size: 1.2em;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(255, 107, 107, 0.3);
            display: block;
            margin: 0 auto;
        }

        .generate-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(255, 107, 107, 0.4);
        }

        .results {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-top: 30px;
        }

        .result-section {
            background: white;
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.08);
            border: 1px solid #e9ecef;
        }

        .result-section h2 {
            color: #333;
            margin-bottom: 20px;
            font-size: 1.8em;
            text-align: center;
            padding-bottom: 10px;
            border-bottom: 3px solid #4facfe;
        }

        .meal-section, .workout-section {
            margin-bottom: 25px;
        }

        .meal-section h3, .workout-section h3 {
            color: #555;
            margin-bottom: 15px;
            font-size: 1.3em;
            display: flex;
            align-items: center;
        }

        .meal-section h3::before {
            content: "🍽️";
            margin-right: 10px;
        }

        .workout-section h3::before {
            content: "💪";
            margin-right: 10px;
        }

        .meal-item, .exercise-item {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 10px;
            border-left: 4px solid #4facfe;
        }

        .meal-item h4, .exercise-item h4 {
            color: #333;
            margin-bottom: 8px;
            font-size: 1.1em;
        }

        .meal-details, .exercise-details {
            color: #666;
            font-size: 0.9em;
            line-height: 1.4;
        }

        .nutrition-summary, .workout-summary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 10px;
            margin-top: 20px;
        }

        .nutrition-summary h4, .workout-summary h4 {
            margin-bottom: 10px;
            font-size: 1.2em;
        }

        .summary-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 15px;
        }

        .summary-item {
            text-align: center;
            background: rgba(255, 255, 255, 0.1);
            padding: 10px;
            border-radius: 8px;
        }

        .summary-item .value {
            font-size: 1.4em;
            font-weight: bold;
            display: block;
        }

        .summary-item .label {
            font-size: 0.9em;
            opacity: 0.9;
        }

        @media (max-width: 768px) {
            .results {
                grid-template-columns: 1fr;
            }
            
            .settings-grid {
                grid-template-columns: 1fr;
            }
            
            .header h1 {
                font-size: 2em;
            }
        }

        .hidden {
            display: none;
        }

        .loading {
            text-align: center;
            padding: 50px;
            color: #666;
            font-size: 1.2em;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎯 智能饮食训练计划生成器</h1>
            <p>基于个人需求，随机生成科学的一日饮食和训练方案</p>
        </div>

        <div class="content">
            <div class="settings-panel">
                <div class="settings-grid">
                    <div class="setting-group">
                        <h3>基本信息</h3>
                        <div class="form-group">
                            <label>性别</label>
                            <select id="gender">
                                <option value="male">男性</option>
                                <option value="female">女性</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label>年龄</label>
                            <input type="number" id="age" value="25" min="15" max="80">
                        </div>
                        <div class="form-group">
                            <label>体重 (kg)</label>
                            <input type="number" id="weight" value="70" min="40" max="150">
                        </div>
                        <div class="form-group">
                            <label>身高 (cm)</label>
                            <input type="number" id="height" value="170" min="140" max="220">
                        </div>
                    </div>

                    <div class="setting-group">
                        <h3>目标设定</h3>
                        <div class="form-group">
                            <label>健身目标</label>
                            <select id="goal">
                                <option value="lose_weight">减脂</option>
                                <option value="gain_muscle">增肌</option>
                                <option value="maintain">保持</option>
                                <option value="endurance">提升耐力</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label>活动水平</label>
                            <select id="activity">
                                <option value="sedentary">久坐不动</option>
                                <option value="light">轻度活动</option>
                                <option value="moderate">中度活动</option>
                                <option value="active">高度活动</option>
                                <option value="very_active">极度活动</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label>目标卡路里 (可选)</label>
                            <input type="number" id="target_calories" placeholder="留空自动计算">
                        </div>
                    </div>

                    <div class="setting-group">
                        <h3>饮食偏好</h3>
                        <div class="form-group">
                            <label>饮食类型</label>
                            <div class="checkbox-group" id="diet-types">
                                <div class="checkbox-item">
                                    <input type="checkbox" id="balanced" checked>
                                    <label for="balanced">均衡饮食</label>
                                </div>
                                <div class="checkbox-item">
                                    <input type="checkbox" id="vegetarian">
                                    <label for="vegetarian">素食</label>
                                </div>
                                <div class="checkbox-item">
                                    <input type="checkbox" id="low_carb">
                                    <label for="low_carb">低碳水</label>
                                </div>
                                <div class="checkbox-item">
                                    <input type="checkbox" id="high_protein">
                                    <label for="high_protein">高蛋白</label>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="setting-group">
                        <h3>训练偏好</h3>
                        <div class="form-group">
                            <label>训练类型</label>
                            <div class="checkbox-group" id="workout-types">
                                <div class="checkbox-item">
                                    <input type="checkbox" id="strength" checked>
                                    <label for="strength">力量训练</label>
                                </div>
                                <div class="checkbox-item">
                                    <input type="checkbox" id="cardio" checked>
                                    <label for="cardio">有氧运动</label>
                                </div>
                                <div class="checkbox-item">
                                    <input type="checkbox" id="flexibility">
                                    <label for="flexibility">柔韧性</label>
                                </div>
                                <div class="checkbox-item">
                                    <input type="checkbox" id="hiit">
                                    <label for="hiit">HIIT</label>
                                </div>
                            </div>
                        </div>
                        <div class="form-group">
                            <label>训练时长 (分钟)</label>
                            <input type="number" id="workout_duration" value="45" min="15" max="120">
                        </div>
                    </div>
                </div>

                <button class="generate-btn" onclick="generatePlan()">🎲 生成今日计划</button>
            </div>

            <div id="results" class="results hidden">
                <div class="result-section">
                    <h2>🍽️ 饮食计划</h2>
                    <div id="meal-plan"></div>
                    <div class="nutrition-summary">
                        <h4>营养总结</h4>
                        <div class="summary-grid" id="nutrition-summary"></div>
                    </div>
                </div>

                <div class="result-section">
                    <h2>💪 训练计划</h2>
                    <div id="workout-plan"></div>
                    <div class="workout-summary">
                        <h4>训练总结</h4>
                        <div class="summary-grid" id="workout-summary"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // 食物数据库
        const foodDatabase = {
            breakfast: [
                { name: "燕麦粥", calories: 150, protein: 5, carbs: 30, fat: 3, tags: ["balanced", "vegetarian"] },
                { name: "全麦面包+鸡蛋", calories: 280, protein: 15, carbs: 35, fat: 8, tags: ["balanced", "high_protein"] },
                { name: "希腊酸奶+坚果", calories: 220, protein: 20, carbs: 15, fat: 12, tags: ["high_protein", "low_carb"] },
                { name: "水果沙拉", calories: 120, protein: 2, carbs: 30, fat: 1, tags: ["vegetarian", "low_carb"] },
                { name: "蛋白质奶昔", calories: 200, protein: 25, carbs: 10, fat: 5, tags: ["high_protein", "low_carb"] },
                { name: "豆浆+全麦包子", calories: 260, protein: 12, carbs: 40, fat: 6, tags: ["vegetarian", "balanced"] }
            ],
            lunch: [
                { name: "鸡胸肉+糙米+蔬菜", calories: 450, protein: 35, carbs: 50, fat: 8, tags: ["balanced", "high_protein"] },
                { name: "三文鱼+藜麦沙拉", calories: 380, protein: 30, carbs: 25, fat: 15, tags: ["high_protein", "low_carb"] },
                { name: "素食汉堡+红薯条", calories: 420, protein: 18, carbs: 55, fat: 12, tags: ["vegetarian", "balanced"] },
                { name: "牛肉+西兰花+土豆", calories: 480, protein: 40, carbs: 35, fat: 18, tags: ["high_protein", "balanced"] },
                { name: "豆腐+蔬菜炒饭", calories: 350, protein: 15, carbs: 45, fat: 10, tags: ["vegetarian", "balanced"] },
                { name: "金枪鱼沙拉", calories: 320, protein: 28, carbs: 15, fat: 18, tags: ["high_protein", "low_carb"] }
            ],
            dinner: [
                { name: "烤鸡+蒸蔬菜", calories: 350, protein: 30, carbs: 20, fat: 15, tags: ["high_protein", "low_carb"] },
                { name: "素食意面", calories: 400, protein: 12, carbs: 65, fat: 8, tags: ["vegetarian", "balanced"] },
                { name: "三文鱼+芦笋", calories: 320, protein: 28, carbs: 10, fat: 18, tags: ["high_protein", "low_carb"] },
                { name: "瘦肉+蔬菜汤", calories: 280, protein: 25, carbs: 15, fat: 12, tags: ["high_protein", "low_carb"] },
                { name: "豆腐蔬菜汤", calories: 200, protein: 12, carbs: 20, fat: 8, tags: ["vegetarian", "low_carb"] },
                { name: "虾+蔬菜沙拉", calories: 250, protein: 22, carbs: 12, fat: 10, tags: ["high_protein", "low_carb"] }
            ],
            snacks: [
                { name: "混合坚果", calories: 160, protein: 6, carbs: 6, fat: 14, tags: ["high_protein", "low_carb"] },
                { name: "苹果+花生酱", calories: 180, protein: 4, carbs: 25, fat: 8, tags: ["balanced"] },
                { name: "蛋白棒", calories: 140, protein: 15, carbs: 12, fat: 4, tags: ["high_protein"] },
                { name: "酸奶", calories: 100, protein: 10, carbs: 12, fat: 2, tags: ["high_protein", "balanced"] },
                { name: "胡萝卜条", calories: 25, protein: 1, carbs: 6, fat: 0, tags: ["vegetarian", "low_carb"] }
            ]
        };

        // 训练动作数据库
        const exerciseDatabase = {
            strength: [
                { name: "俯卧撑", duration: 3, sets: 3, reps: "12-15", calories: 8, equipment: "无器械", muscle: ["胸肌", "三头肌"] },
                { name: "深蹲", duration: 4, sets: 3, reps: "15-20", calories: 10, equipment: "无器械", muscle: ["腿部", "臀部"] },
                { name: "引体向上", duration: 3, sets: 3, reps: "8-12", calories: 9, equipment: "单杠", muscle: ["背肌", "二头肌"] },
                { name: "哑铃卧推", duration: 4, sets: 4, reps: "10-12", calories: 12, equipment: "哑铃", muscle: ["胸肌"] },
                { name: "硬拉", duration: 5, sets: 3, reps: "8-10", calories: 15, equipment: "杠铃", muscle: ["背肌", "腿部"] },
                { name: "肩上推举", duration: 3, sets: 3, reps: "10-12", calories: 8, equipment: "哑铃", muscle: ["肩膀"] }
            ],
            cardio: [
                { name: "跑步", duration: 20, sets: 1, reps: "20分钟", calories: 200, equipment: "无器械", muscle: ["全身"] },
                { name: "跳绳", duration: 10, sets: 3, reps: "3分钟", calories: 120, equipment: "跳绳", muscle: ["全身"] },
                { name: "单车", duration: 25, sets: 1, reps: "25分钟", calories: 180, equipment: "单车", muscle: ["腿部"] },
                { name: "游泳", duration: 30, sets: 1, reps: "30分钟", calories: 250, equipment: "泳池", muscle: ["全身"] },
                { name: "椭圆机", duration: 20, sets: 1, reps: "20分钟", calories: 160, equipment: "椭圆机", muscle: ["全身"] }
            ],
            hiit: [
                { name: "波比跳", duration: 4, sets: 4, reps: "30秒", calories: 15, equipment: "无器械", muscle: ["全身"] },
                { name: "高抬腿", duration: 3, sets: 4, reps: "30秒", calories: 12, equipment: "无器械", muscle: ["腿部"] },
                { name: "山地跑", duration: 3, sets: 4, reps: "30秒", calories: 10, equipment: "无器械", muscle: ["核心", "腿部"] },
                { name: "跳跃深蹲", duration: 3, sets: 4, reps: "30秒", calories: 12, equipment: "无器械", muscle: ["腿部"] },
                { name: "平板撑", duration: 2, sets: 3, reps: "45秒", calories: 6, equipment: "无器械", muscle: ["核心"] }
            ],
            flexibility: [
                { name: "瑜伽拉伸", duration: 15, sets: 1, reps: "15分钟", calories: 30, equipment: "瑜伽垫", muscle: ["全身"] },
                { name: "腿部拉伸", duration: 8, sets: 1, reps: "8分钟", calories: 15, equipment: "无器械", muscle: ["腿部"] },
                { name: "肩颈拉伸", duration: 5, sets: 1, reps: "5分钟", calories: 10, equipment: "无器械", muscle: ["肩颈"] },
                { name: "脊椎扭转", duration: 6, sets: 1, reps: "6分钟", calories: 12, equipment: "无器械", muscle: ["背部"] }
            ]
        };

        // 初始化复选框事件
        document.addEventListener('DOMContentLoaded', function() {
            const checkboxGroups = document.querySelectorAll('.checkbox-group');
            checkboxGroups.forEach(group => {
                const checkboxItems = group.querySelectorAll('.checkbox-item');
                checkboxItems.forEach(item => {
                    const checkbox = item.querySelector('input[type="checkbox"]');
                    
                    item.addEventListener('click', function(e) {
                        if (e.target.type !== 'checkbox') {
                            checkbox.checked = !checkbox.checked;
                        }
                        
                        if (checkbox.checked) {
                            item.classList.add('checked');
                        } else {
                            item.classList.remove('checked');
                        }
                    });
                    
                    // 初始状态
                    if (checkbox.checked) {
                        item.classList.add('checked');
                    }
                });
            });
        });

        // 计算基础代谢率
        function calculateBMR(gender, age, weight, height) {
            if (gender === 'male') {
                return 88.362 + (13.397 * weight) + (4.799 * height) - (5.677 * age);
            } else {
                return 447.593 + (9.247 * weight) + (3.098 * height) - (4.330 * age);
            }
        }

        // 计算每日消耗卡路里
        function calculateTDEE(bmr, activity) {
            const multipliers = {
                sedentary: 1.2,
                light: 1.375,
                moderate: 1.55,
                active: 1.725,
                very_active: 1.9
            };
            return bmr * multipliers[activity];
        }

        // 根据目标调整卡路里
        function adjustCaloriesForGoal(tdee, goal) {
            switch (goal) {
                case 'lose_weight':
                    return tdee - 500; // 减脂：减少500卡路里
                case 'gain_muscle':
                    return tdee + 300; // 增肌：增加300卡路里
                case 'maintain':
                    return tdee; // 保持
                case 'endurance':
                    return tdee + 200; // 耐力：增加200卡路里
                default:
                    return tdee;
            }
        }

        // 过滤食物
        function filterFoods(foods, dietTypes) {
            if (dietTypes.length === 0) return foods;
            return foods.filter(food => 
                dietTypes.some(type => food.tags.includes(type))
            );
        }

        // 过滤训练动作
        function filterExercises(exercises, workoutTypes) {
            if (workoutTypes.length === 0) return exercises;
            let filtered = [];
            workoutTypes.forEach(type => {
                if (exerciseDatabase[type]) {
                    filtered.push(...exerciseDatabase[type]);
                }
            });
            return filtered;
        }

        // 随机选择元素
        function getRandomItems(array, count) {
            const shuffled = [...array].sort(() => 0.5 - Math.random());
            return shuffled.slice(0, count);
        }

        // 生成饮食计划
        function generateMealPlan(targetCalories, dietTypes) {
            const calorieDistribution = {
                breakfast: 0.25,
                lunch: 0.35,
                dinner: 0.30,
                snacks: 0.10
            };

            const mealPlan = {};
            let totalCalories = 0;
            let totalProtein = 0;
            let totalCarbs = 0;
            let totalFat = 0;

            Object.keys(calorieDistribution).forEach(mealType => {
                const targetMealCalories = targetCalories * calorieDistribution[mealType];
                const availableFoods = filterFoods(foodDatabase[mealType], dietTypes);
                
                if (availableFoods.length > 0) {
                    let selectedFoods = [];
                    let mealCalories = 0;
                    
                    // 为零食选择1-2个，其他餐选择1个主要食物
                    const numItems = mealType === 'snacks' ? Math.random() > 0.5 ? 2 : 1 : 1;
                    const foods = getRandomItems(availableFoods, numItems);
                    
                    foods.forEach(food => {
                        selectedFoods.push({...food});
                        mealCalories += food.calories;
                        totalCalories += food.calories;
                        totalProtein += food.protein;
                        totalCarbs += food.carbs;
                        totalFat += food.fat;
                    });
                    
                    mealPlan[mealType] = {
                        foods: selectedFoods,
                        calories: mealCalories
                    };
                }
            });

            return {
                meals: mealPlan,
                nutrition: {
                    calories: Math.round(totalCalories),
                    protein: Math.round(totalProtein),
                    carbs: Math.round(totalCarbs),
                    fat: Math.round(totalFat)
                }
            };
        }

        // 生成训练计划
        function generateWorkoutPlan(workoutTypes, duration) {
            const availableExercises = filterExercises([], workoutTypes);
            
            if (availableExercises.length === 0) {
                return { exercises: [], summary: { duration: 0, calories: 0, exercises: 0 } };
            }

            let selectedExercises = [];
            let totalDuration = 0;
            let totalCalories = 0;

            // 根据时长选择练习
            while (totalDuration < duration && availableExercises.length > 0) {
                const remainingTime = duration - totalDuration;
                const suitableExercises = availableExercises.filter(ex => ex.duration <= remainingTime);
                
                if (suitableExercises.length === 0) break;
                
                const exercise = suitableExercises[Math.floor(Math.random() * suitableExercises.length)];
                selectedExercises.push({...exercise});
                totalDuration += exercise.duration;
                totalCalories += exercise.calories;
                
                // 移除已选择的练习，避免重复
                const index = availableExercises.indexOf(exercise);
                availableExercises.splice(index, 1);
            }

            return {
                exercises: selectedExercises,
                summary: {
                    duration: totalDuration,
                    calories: Math.round(totalCalories),
                    exercises: selectedExercises.length
                }
            };
        }

        // 生成计划主函数
        function generatePlan() {
            // 获取用户输入
            const gender = document.getElementById('gender').value;
            const age = parseInt(document.getElementById('age').value);
            const weight = parseInt(document.getElementById('weight').value);
            const height = parseInt(document.getElementById('height').value);
            const goal = document.getElementById('goal').value;
            const activity
