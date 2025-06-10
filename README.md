# Fuzzy-interference-
# fuzzy_inference_system.py

def fuzzify_temperature(temp):
    cold = max(0, min(1, (25 - temp) / 10))
    warm = max(0, min((temp - 20) / 5, (30 - temp) / 5))
    hot = max(0, min(1, (temp - 25) / 10))
    return {"cold": cold, "warm": warm, "hot": hot}

def apply_rules(temp_fuzzy):
    rules = {
        "low": temp_fuzzy["cold"],
        "medium": temp_fuzzy["warm"],
        "high": temp_fuzzy["hot"]
    }
    return rules

def defuzzify(output_fuzzy):
    # Centroid Method
    num = 0
    den = 0
    for speed, degree in output_fuzzy.items():
        if speed == "low":
            value = 30
        elif speed == "medium":
            value = 60
        elif speed == "high":
            value = 90
        num += value * degree
        den += degree
    return num / den if den != 0 else 0

def fuzzy_inference(temp):
    temp_fuzzy = fuzzify_temperature(temp)
    rule_outputs = apply_rules(temp_fuzzy)
    crisp_output = defuzzify(rule_outputs)
    return crisp_output

# --------- Example ---------

if __name__ == "__main__":
    temperature = float(input("Enter room temperature (°C): "))
    fan_speed = fuzzy_inference(temperature)
    print(f"Recommended Fan Speed: {fan_speed:.2f} RPM")
