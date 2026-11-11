# onifade-michael_resistor_calculator
# Resistor combination calculator 
# Calculates total resistance in series and in parallel.
# Own twist: also calculates current and power if the user gives a supply voltage.


def get_whole_number(prompt):
    """Keep asking until the user types a whole number greater than 0."""
    while True:
        text = input(prompt)
        try:
            number = int(text)  # convert text to a whole number
        except ValueError:
            print("Please type a whole number, for example 3.")
            continue
        if number < 1:
            print("The number must be at least 1.")
        else:
            return number


def get_positive_float(prompt):
    """Keep asking until the user types a number greater than 0."""
    while True:
        text = input(prompt)
        try:
            number = float(text)  # convert text to a decimal number
        except ValueError:
            print("That is not a number. Please try again, for example 220 or 4.7.")
            continue
        if number <= 0:
            print("A resistor value must be greater than 0.")
        else:
            return number


def series_total(values):
    """Series: just add all the resistances."""
    return sum(values)


def parallel_total(values):
    """Parallel: 1/Rt = 1/R1 + 1/R2 + ... so add the reciprocals, then flip."""
    reciprocal_sum = 0
    for r in values:
        reciprocal_sum += 1 / r
    return 1 / reciprocal_sum


def show_circuit_details(name, resistance, voltage):
    """Twist: use Ohm's law (I = V / R and P = V x I) for a circuit."""
    current = voltage / resistance
    power = voltage * current
    print(f"  {name}: current = {current:.4f} A, power = {power:.4f} W")


def main():
    print("=== Resistor Combination Calculator ===")

    count = get_whole_number("How many resistors do you have? ")

    # Store every resistor value in a list
    resistors = []
    for i in range(1, count + 1):
        value = get_positive_float(f"Enter value of resistor {i} in ohms: ")
        resistors.append(value)

    total_series = series_total(resistors)
    total_parallel = parallel_total(resistors)

    print("\n--- Results ---")
    print(f"Resistors entered: {resistors}")
    print(f"Total in series:   {total_series:.2f} ohms")
    print(f"Total in parallel: {total_parallel:.2f} ohms")

    # Decision: a short comment on the result
    if count == 1:
        print("With only one resistor, series and parallel are the same.")
    elif total_parallel < min(resistors):
        print("Note: parallel total is smaller than your smallest resistor.")
    else:
        print("Check your values, parallel should be below the smallest resistor.")

    # Twist: optional supply voltage
    answer = input("\nDo you want current and power? Type y for yes: ").lower()
    if answer == "y":
        voltage = get_positive_float("Enter supply voltage in volts: ")
        print("\nWith that supply voltage:")
        show_circuit_details("Series", total_series, voltage)
        show_circuit_details("Parallel", total_parallel, voltage)
    else:
        print("Okay, skipping current and power.")


main()