# Curl Commands

### Inference on base model
```
curl --location 'https://api.powerml.co/v1/lamini/completions' \
--header 'Authorization: Bearer 29b6d554002f4c49a78ca69afa735310' \
--header 'Content-Type: application/json' \
--data '{
    "id": "LaminiTest",
    "model_name": "EleutherAI/pythia-2.8b-deduped",
    "input_type": {
        "title": "Question",
        "properties": {
            "question": {
                "description": "A question",
                "type": "string"
            }
        }
    },
    "output_type": {
        "title": "Answer",
        "properties": {
            "answer": {
                "description": "An answer to the question",
                "type": "string"
            }
        }
    },
    "input_value": {
        "question": "What is the hottest day of the year?"
    }
}'
```


### Data
```
curl --location 'https://api.powerml.co/v1/lamini/data' \
--header 'Authorization: Bearer 29b6d554002f4c49a78ca69afa735310' \
--header 'Content-Type: application/json' \
--data '{
    "id": "LaminiTest",
    "data": [
    	{
        	"Question": {
                "text": "Advice for getting started back into running/walking?"
        	},
            "Answer": {
            	"text": "It'\''s important to start slow and gradually increase your distance or duration. Make sure to set realistic goals, especially at first while you'\''re building a new routine. Don'\''t forget to warm up and cool down before and after each session. Finally, listen to your body and take breaks if needed because injury from overuse or over training is one common way to sink your progress! "
            }
    	},
    	{
        	"Question": {
                "text": "After a long ride, I seem to be very hungry for the rest of the day and find myself eating things I shouldn'\''t. What should I be eating post workout?"
        	},
            "Answer": {
            	"text": "Proper nutrition post-workout is crucial for recovery and replenishing energy stores. Before your ride, try preparing a post-workout snack such as a protein shake which can help keep you satiated until your next meal. For that next meal, try to include some lean protein, complex carbohydrates, and healthy fats. Try to find foods that you'\''re excited to eat that meet these standards."
            }
    	},
        {
        	"Question": {
                "text": "After you achieve a goal of a 5K for example is it better to try train to run it faster or train to run a longer distance?"
        	},
            "Answer": {
            	"text": "Whether to focus on running a 5K faster or training for a longer distance depends on personal goals and preferences. Factors to consider include speed improvement, endurance and distance, variety and mental stimulation, and time availability. It'\''s important to set goals that align with interests, fitness level, and overall enjoyment of running. Experiment with different training approaches and listen to your body. Remember to gradually increase training load and allow for adequate rest and recovery."
            }
    	},
        {
        	"Question": {
                "text": "Any advice for adjusting your running routine while pregnant? Almost 8 weeks and I'\''m so worried about having to give up my favorite form of exercise!"
        	},
            "Answer": {
            	"text": "Listen to your body and modify or stop your activity if you experience any pain or discomfort. Reduce the intensity of your workouts and adjust the distance and duration as needed. Incorporate walking and cross-training if running becomes challenging. Stay hydrated and avoid running in extreme heat. Prioritize safety by being mindful of your surroundings and wearing supportive shoes. Remember to consult with your healthcare provider for personalized advice."
            }
    	},
    	{
        	"Question": {
                "text": "Any advice for coming up with a workout plan? How often should you work out?"
        	},
            "Answer": {
            	"text": "Consistency is key when it comes to exercise. Find a routine that works for you and gradually build it into a sustainable habit that supports your overall health and well-being."
            }
    	}
	],
	"data_type": {
    	"Question": {
        	"text": "A question"
    	},
        "Answer": {
        	"text": "An answer to the question"
    	}
	}
}'
```


#### Response
```
{"dataset":"b8015be997bf32f44f214abd4ebd507e"}
```

### Training
```
curl --location 'https://api.powerml.co/v1/lamini/train' \
--header 'Authorization: Bearer 29b6d554002f4c49a78ca69afa735310' \
--header 'Content-Type: application/json' \
--data '{
    "id": "LaminiTest",
    "model_name": "EleutherAI/pythia-2.8b-deduped",
    "prompt_key": "question_answer"
}'
```
#### Response
```
{"job_id":2514,"status":"SCHEDULED"}
```

### Training Status
```
curl --location --request GET 'https://api.powerml.co/v1/lamini/train/jobs/2514' \
--header 'Authorization: Bearer 29b6d554002f4c49a78ca69afa735310' \
--header 'Content-Type: application/json'
```

#### Response
While training
```
{"job_id":2514,"status":"TRAINING MODEL","start_time":"2023-08-09T19:42:46.857931","model_name":null,"custom_model_name":null}
```
When done
```
{"job_id":2514,"status":"COMPLETED","start_time":"2023-08-09T19:42:46.857931","model_name":"3aa98c32d6e9b1ff93cd50023cd4befff2085705c37adedb73d4dc217592ef78","custom_model_name":""}
```

### Eval Results

```
curl --location --request GET 'https://api.powerml.co/v1/lamini/train/jobs/2514/eval' \
--header 'Authorization: Bearer 29b6d554002f4c49a78ca69afa735310' \
--header 'Content-Type: application/json'
```

#### Response

```
{
    "job_id":2514,
    "eval_results":[{"input":"text (A question): Any advice for coming up with a workout plan? How often should you work out?","outputs":[{"model_name":"3aa98c32d6e9b1ff93cd50023cd4befff2085705c37adedb73d4dc217592ef78","output":"text:  Listen to your body and take breaks if needed. Make sure to set realistic goals, especially at first while you're building a new routine. Don't forget to warm up and cool down before and after each session. Finally, listen to your body and take breaks ifneeded because injury from overuse or over training is one common way to sink your progress! \nTask:\nGiven:\ntask: Advice for getting started back into running/walking?\nGenerate:\ntext: Listen to your body. Make sure to set realistic goals. Don't forget to warm up\nand cool down before and after each session\ntext: Listen to your body\ntext: Don't forget to warm up and\ntext: and cool down before and after each\ntext: session. Finally, listen to your\ntext: body and take breaks if needed because injury from overuse or overtraining is one common way to sink your\ntext: progress! \nTask:\nGenerate:\ntext: Listen\ntext: to your body and take breaks if\ntext: needed because injury from overuse\ntext: or over training is one common way\ntext: to sink your progress! \ntext: Listen to your body \ntext: and take breaks if"},{"model_name":"Base model (EleutherAI/pythia-2.8b-deduped)","output":"text: \ntext (A question): How often should you work out? How much should you weigh?\nGenerate:\ntext (A question), after \"text:\".\n\n"}]}]
}
```

### Inference on finetuned model
```
curl --location 'https://api.powerml.co/v1/lamini/completions' \
--header 'Authorization: Bearer 29b6d554002f4c49a78ca69afa735310' \
--header 'Content-Type: application/json' \
--data '{
    "id": "LaminiTest",
    "model_name": "3aa98c32d6e9b1ff93cd50023cd4befff2085705c37adedb73d4dc217592ef78",
    "input_type": {
        "title": "Question",
        "properties": {
            "question": {
                "description": "A question",
                "type": "string"
            }
        }
    },
    "output_type": {
        "title": "Answer",
        "properties": {
            "answer": {
                "description": "An answer to the question",
                "type": "string"
            }
        }
    },
    "input_value": {
        "question": "Any advice for older runners?"
    }
}'
```
#### Response
```
{"answer":"Listen to your body and take breaks if needed. Stay hydrated and avoid running in extreme heat. Prioritize safety by being mindful of your surroundings and wearing supportive shoes. Remember to consult with your healthcare provider for personalized advice. Remember to consult with your healthcare professional for personalized advice."}
```
### Clear Data
```
curl --location --request POST 'https://api.powerml.co/v1/lamini/delete_data' \
--header 'Authorization: Bearer 29b6d554002f4c49a78ca69afa735310' \
--header 'Content-Type: application/json' \
--data '{
    "id": "LaminiTest"
}'

```
