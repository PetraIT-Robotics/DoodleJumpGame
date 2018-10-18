# DoodleJumpGame

#include <SFML/Graphics.hpp>
#include <time.h>
#include <iostream>

#include "Player.h"

using namespace sf;

struct point
{
	int x, y;
};

void restart() 
{
	std::exit(42);
}


int main()
{
	srand(time(0));

	RenderWindow app(VideoMode(400, 533), "Doodle Game!");
	app.setFramerateLimit(60);

	Texture t1, t2, t3;
	t1.loadFromFile("images/background.png");
	t2.loadFromFile("images/platform.png");
	t3.loadFromFile("images/doodle.png");

	Sprite sBackground(t1), sPlat(t2), sPers(t3);

	Font font;
	font.loadFromFile("font/arial.ttf");

	Text tx_over,tx_enter,tx_score,tx_WordScore;
	tx_over.setFont(font);
	tx_over.setString("Game Over");
	tx_over.setCharacterSize(70);
	tx_over.setFillColor(Color::Red);
	tx_over.setStyle(Text::Bold);
	tx_over.setPosition(18, 200);

	tx_enter.setFont(font);
	tx_enter.setString("Press Enter to Exit");
	tx_enter.setCharacterSize(30);
	tx_enter.setFillColor(Color::Black);
	tx_enter.setStyle(Text::Underlined);
	tx_enter.setPosition(70, 280);

	tx_score.setFont(font);
	tx_score.setString("0");
	tx_score.setCharacterSize(40);
	tx_score.setFillColor(Color::Black);
	tx_score.setStyle(Text::Bold);
	tx_score.setPosition(130, 10);

	tx_WordScore.setFont(font);
	tx_WordScore.setString("Score: ");
	tx_WordScore.setCharacterSize(40);
	tx_WordScore.setFillColor(Color::Black);
	tx_WordScore.setStyle(Text::Bold);
	tx_WordScore.setPosition(3, 10);


	point plat[20];

	for (int i = 0; i < 10; i++)
	{
		plat[i].x = rand() % 400;
		plat[i].y = rand() % 533;
	}

	app.draw(tx_WordScore);
	app.display();

	int x = 100, y = 100, h = 200, oldy = 0, distance = 0;
	float dx = 0, dy = 0;
	long long score = 0;

	while (app.isOpen())
	{
		Event e;
		while (app.pollEvent(e))
		{
			if (e.type == Event::Closed)
				app.close();
		}

		if (Keyboard::isKeyPressed(Keyboard::Right)) x += 3;
		if (Keyboard::isKeyPressed(Keyboard::Left)) x -= 3;
		dy += 0.2;
		y += dy;


		if (y < h + 3)
			for (int i = 0; i < 10; i++)
			{
				y = h;
				plat[i].y = plat[i].y - dy;
				if (plat[i].y > 533) { plat[i].y = 0; plat[i].x = rand() % 400; }
			}

		if (y > 500)
			{
				app.draw(tx_over);
				app.draw(tx_enter);
				sPers.setPosition(x, y);
				app.draw(sPers);
				app.display();
				bool k = 0;
				while(k==0)
					if (Keyboard::isKeyPressed(Keyboard::Enter))
					{
						restart();
					}
			}

		

		for (int i = 0; i < 10; i++)
			if ((x + 50 > plat[i].x) && (x + 20 < plat[i].x + 68)
				&& (y + 70 > plat[i].y) && (y + 70 < plat[i].y + 14) && (dy > 0))
			{
				dy = -10;
				if (plat[distance].y > plat[i].y)
				{
					score += plat[distance].y - plat[i].y;
				}
				distance = i;
			}
		tx_score.setString(std::to_string(score));

		sPers.setPosition(x, y);

		app.draw(sBackground);
		app.draw(sPers);
		for (int i = 0; i < 10; i++)
		{
			sPlat.setPosition(plat[i].x, plat[i].y);
			app.draw(sPlat);
		}
		app.draw(tx_WordScore);
		app.draw(tx_score);
		app.display();
	}

	return 0;
}
