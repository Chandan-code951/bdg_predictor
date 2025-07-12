
# bdg_predictor
A simple python script to predict Big Daddy Game results# BDG Game Predictor - Pattern Based

# Sample BDG History Data (last 10 entries)
bdg_history = [
    {"number": 7, "big_small": "Big", "color": "Green"},
    {"number": 9, "big_small": "Big", "color": "Green"},
    {"number": 6, "big_small": "Big", "color": "Red"},
    {"number": 4, "big_small": "Small", "color": "Red"},
    {"number": 7, "big_small": "Big", "color": "Green"},
    {"number": 9, "big_small": "Big", "color": "Green"},
    {"number": 0, "big_small": "Small", "color": "Red/Violet"},
    {"number": 6, "big_small": "Big", "color": "Red"},
    {"number": 9, "big_small": "Big", "color": "Green"},
    {"number": 2, "big_small": "Small", "color": "Red"}
]

# Helper function to classify number
def classify_number(num):
    if num >= 5:
        return "Big"
    else:
        return "Small"

def predict_next(history):
    last_numbers = [entry["number"] for entry in history]
    last_big_small = [entry["big_small"] for entry in history]
    last_colors = [entry["color"] for entry in history]

    # Big/Small prediction
    recent_big_small = last_big_small[-6:]
    big_count = recent_big_small.count("Big")
    small_count = recent_big_small.count("Small")
    if big_count > small_count:
        next_bs = "Small"
    else:
        next_bs = "Big"

    # Color prediction
    red = sum("Red" in c for c in last_colors[-6:])
    green = sum("Green" in c for c in last_colors[-6:])
    violet = sum("Violet" in c for c in last_colors[-6:])
    next_color = max([(red, "Red"), (green, "Green"), (violet, "Violet")])[1]

    # Number prediction
    avg = sum(last_numbers[-5:]) / 5
    if avg >= 7:
        next_number = 8
    elif avg >= 5:
        next_number = 6
    elif avg >= 3:
        next_number = 4
    else:
        next_number = 2

    return {
        "predicted_number": next_number,
        "predicted_big_small": classify_number(next_number),
        "predicted_color": next_color
    }

# Run prediction
prediction = predict_next(bdg_history)
print("Next Prediction:")
print(prediction) 
