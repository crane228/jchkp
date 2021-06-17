#include <iostream>

class Spot {
	int count_ = 0;

public:
	bool hidden = true;
	int get_count() {
		return count_;
	}

	void push() {
		++count_;
	}
};

class Field {
	Spot** spots_[10];
public:
	Field() {
		for (int i = 0; i < 10; i++) {
			spots_[i] = new Spot * [10];
			for (int j = 0; j < 10; j++) {
				spots_[i][j] = new Spot();

			}
		}
		fill();
	}

	void fill() {
		for (int i = 0; i < 8; i++) {
			int rand_x = rand() % 10, rand_y = rand() % 10;
			spots_[rand_x][rand_y]->push();
			std::cout << rand_x << ' ' << rand_y << std::endl;
		}
	}

	Spot*** get_spots() {
		return spots_;
	}

	void draw() {
		system("cls");
		for (int j = 9; j >= 0; j--) {
			for (int i = 0; i < 10; i++) {
				if (spots_[i][j]->hidden) std::cout << '-';
				else std::cout << spots_[i][j]->get_count();
				std::cout << ' ';
			}

			std::cout << "| " << j + 1 << std::endl;
		}
		for (int j = 0; j < 20; j++) {
			std::cout << '-';
		}
		std::cout << std::endl;
		for (int j = 0; j < 10; j++) {
			std::cout << j + 1 << ' ';
		}
		std::cout << std::endl;
	}
};

class Game {
	Field* field_;
public:
	void draw() {
		field_->draw();
	}

	Game() {
		field_ = new Field;
	}

	int look(int x, int y) {
		--x; --y;
		Spot*** spots = field_->get_spots();
		int foxes_seen = 0;
		int j = y + x + 10, i = -10;

		while (i < 9 || j > 0) {
			i++;
			j--;
			if (i == x and j == y) {
				for (int k = 0; k < 10; k++) {
					foxes_seen += spots[x][k]->get_count();
					if (spots[x][k]->get_count()) spots[x][k]->hidden = false;
				}
			}
			else {
				if (i <= 9 && i >= 0) { foxes_seen += spots[i][y]->get_count(); if (spots[i][y]->get_count()) spots[i][y]->hidden = false; }
				if (i <= 9 && i >= 0 && j <= 9 && j >= 0) { foxes_seen += spots[i][j]->get_count(); if (spots[i][j]->get_count()) spots[i][j]->hidden = false; }
				if (2 * y - j >= 0 && i <= 9 && 2 * y - j <= 9 && i >= 0) { foxes_seen += spots[i][2 * y - j]->get_count(); if (spots[i][2 * y - j]->get_count()) spots[i][2 * y - j]->hidden = false; }

			}
			//std::cout << i + 1 << ' ' << j + 1 << ' ' << foxes_seen << std::endl;

		}
		return foxes_seen;
	}
};

int main()
{
	Game game;
	int x = 0, y = 0;
	game.draw();
	int res;
	while (true) {

		std::cin >> x >> y;
		res = game.look(x, y);
		game.draw();
		std::cout << res << std::endl;

	}

	return 0;
}
