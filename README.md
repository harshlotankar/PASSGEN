
import secrets
import string
import time


SYMBOLS = "!@#$%^&*?-_+"


def show_banner():
    print("\n" + "=" * 50)
    print("W E L C O M E T O P A S S G E N".center(50))
    print("=" * 50)
    print("  Let's create a strong password for you!")
    print("=" * 50)


def build_characters(upper, digits, symbols):
    chars = string.ascii_lowercase

    if upper == "y":
        chars += string.ascii_uppercase
    if digits == "y":
        chars += string.digits
    if symbols == "y":
        chars += SYMBOLS

    return chars


def make_password(chars, length):
    return "".join(secrets.choice(chars) for _ in range(length))


def check_strength(password):
    score = 0

    if len(password) >= 8:
        score += 1
    if len(password) >= 12:
        score += 1
    if any(c.isupper() for c in password):
        score += 1
    if any(c.isdigit() for c in password):
        score += 1
    if any(c in SYMBOLS for c in password):
        score += 1

    if score <= 2:
        strength = "Weak"
    elif score == 3:
        strength = "Medium"
    elif score == 4:
        strength = "Strong"
    else:
        strength = "Super Strong"

    bar = "[" + "#" * score + "-" * (5 - score) + "]"
    return strength, bar


def loading():
    print("\nGenerating", end="", flush=True)
    for _ in range(3):
        time.sleep(0.4)
        print(" .", end="", flush=True)
    print("\n")


def get_integer(prompt, minimum, maximum):
    while True:
        try:
            value = int(input(prompt))
            if minimum <= value <= maximum:
                return value
            print(f"Please enter a number from {minimum} to {maximum}.")
        except ValueError:
            print("Invalid input. Please enter a whole number.")


def get_yes_no(prompt):
    while True:
        answer = input(prompt).strip().lower()
        if answer in ("y", "n"):
            return answer
        print("Please enter y or n.")


def main():
    show_banner()

    name = input("\nWhat's your name? ").strip()
    if not name:
        name = "User"

    print(f"\nNice to meet you, {name}! Let's get started.")

    while True:
        length = get_integer(
            "\nHow long should the password be? (4-50): ", 4, 50
        )

        print("\nChoose what to include:")
        upper = get_yes_no("Uppercase letters? (y/n): ")
        digits = get_yes_no("Numbers?           (y/n): ")
        symbols = get_yes_no("Symbols?           (y/n): ")

        count = get_integer(
            "\nHow many passwords do you want? (1-20): ", 1, 20
        )

        chars = build_characters(upper, digits, symbols)

        loading()

        print("-" * 50)
        print("YOUR PASSWORDS".center(50))
        print("-" * 50)

        for i in range(count):
            password = make_password(chars, length)
            strength, bar = check_strength(password)

            print(f"\n{i + 1}. {password}")
            print(f"   Strength: {strength} {bar}")

        again = get_yes_no(
            "\nGenerate more passwords? (y/n): "
        )

        if again != "y":
            print(f"\nGoodbye {name}! Stay safe online.\n")
            break


if __name__ == "__main__":
    main()
