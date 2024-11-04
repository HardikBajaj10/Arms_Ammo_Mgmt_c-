#include <iostream>
#include <string>
using namespace std;

class Weapon {
public:
    string name;
    int stock;

    Weapon(string n = "Unknown Weapon", int s = 0) {
        name=n;
        stock=s;
    }
    virtual void displayInfo() const {
        cout << "Weapon: " << name << ", Stock: " << stock << endl;
    }
    virtual void addStock(int amount) {
        stock += amount;
        cout << amount << " units added. Updated stock: " << stock << endl;
    }
    virtual void removeStock(int amount) {
        if (stock >= amount) {
            stock -= amount;
            cout << amount << " units removed. Updated stock: " << stock << endl;
        } else {
            cout << "Insufficient stock. Only " << stock << " units available." << endl;
        }
    }
    virtual ~Weapon() {}
};
class Firearm : public Weapon {
public:
    int range;

    Firearm(string n = "Unknown Firearm", int s = 0, int r = 0) : Weapon(n, s), range(r) {}

    void displayInfo() const override {
        cout << "Firearm: " << name << ", Stock: " << stock << ", Range: " << range << " meters" << endl;
    }
};
class Explosive : public Weapon {
public:
    int power;
    Explosive(string n = "Unknown Explosive", int s = 0, int p = 0) : Weapon(n, s), power(p) {}

    void displayInfo() const override {
        cout << "Explosive: " << name << ", Stock: " << stock << ", Power: " << power << " kilotons" << endl;
    }
};
// Function prototypes
void addWeapon(Weapon*& weapon);
void updateWeapon(Weapon*& weapon);
void manageStock(Weapon*& weapon);
void displayWeapons(Weapon* weapons[], int numWeapons);

int main() {
    const int MAX_WEAPONS = 5;
    Weapon* weapons[MAX_WEAPONS];
    int weaponCount = 0;
    int choice;

    do {
        cout << "\n--- Arms and Ammunition Management System ---" << endl;
        cout << "1. Add a new weapon" << endl;
        cout << "2. Update existing weapon details" << endl;
        cout << "3. Add or remove stock" << endl;
        cout << "4. Display all weapons" << endl;
        cout << "5. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                if (weaponCount < MAX_WEAPONS) {
                    addWeapon(weapons[weaponCount]);
                    weaponCount++;
                } else {
                    cout << "Maximum weapon limit reached!" << endl;
                }
                break;

            case 2:
                if (weaponCount > 0) {
                    updateWeapon(weapons[weaponCount - 1]);
                } else {
                    cout << "No weapons available to update!" << endl;
                }
                break;

            case 3:
                if (weaponCount > 0) {
                    manageStock(weapons[weaponCount - 1]);
                } else {
                    cout << "No weapons available to manage stock!" << endl;
                }
                break;

            case 4:
                if (weaponCount > 0) {
                    displayWeapons(weapons, weaponCount);
                } else {
                    cout << "No weapons to display!" << endl;
                }
                break;

            case 5:
                cout << "Exiting the system..." << endl;
                break;

            default:
                cout << "Invalid choice. Please try again." << endl;
        }
    } while (choice != 5);

    // Cleanup memory before exiting
    for (int i = 0; i < weaponCount; i++) {
        delete weapons[i];
    }
    return 0;
}
// Function to add a new weapon
void addWeapon(Weapon*& weapon) {
    int type;
    cout << "\nEnter weapon type (1 for Firearm, 2 for Explosive): ";
    cin >> type;

    string name;
    int stock;
    cout << "Enter weapon name: ";
    cin.ignore();
    getline(cin, name);
    cout << "Enter initial stock: ";
    cin >> stock;

    if (type == 1) {
        int range;
        cout << "Enter range (in meters): ";
        cin >> range;
        weapon = new Firearm(name, stock, range);
    } else if (type == 2) {
        int power;
        cout << "Enter power (in kilotons): ";
        cin >> power;
        weapon = new Explosive(name, stock, power);
    } else {
        cout << "Invalid weapon type!" << endl;
    }

    cout << "\nWeapon added successfully!" << endl;
}
// Function to update existing weapon details
void updateWeapon(Weapon*& weapon) {
    string newName;
    int newStock;

    cout << "\nUpdating weapon details..." << endl;
    cout << "Enter new weapon name: ";
    cin.ignore();
    getline(cin, newName);
    cout << "Enter new stock: ";
    cin >> newStock;

    weapon->name = newName;
    weapon->stock = newStock;

    cout << "Weapon details updated!" << endl;
}

// Function to manage stock (add/remove)
void manageStock(Weapon*& weapon) {
    int choice, amount;
    cout << "\nManage Stock:" << endl;
    cout << "1. Add stock" << endl;
    cout << "2. Remove stock" << endl;
    cout << "Enter your choice: ";
    cin >> choice;

    if (choice == 1) {
        cout << "Enter the amount to add: ";
        cin >> amount;
        weapon->addStock(amount);
    } else if (choice == 2) {
        cout << "Enter the amount to remove: ";
        cin >> amount;
        weapon->removeStock(amount);
    } else {
        cout << "Invalid option!" << endl;
    }
}

// Function to display all weapons
void displayWeapons(Weapon* weapons[], int numWeapons) {
    cout << "\nDisplaying all weapons:" << endl;
    for (int i = 0; i < numWeapons; i++) {
        weapons[i]->displayInfo();
    }
}
