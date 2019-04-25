#include <iostream>
#include <cmath>
#include <cstdlib>
#include <fstream>
#include <string>
#include <sstream>
#include <vector>
#include <numeric>
using namespace std;

void displayWelcome();
// Displays the initial welcome menu

void writeOut(ofstream& outFile, int job, double length, double sHeight, double eHeight, char local, char access, char condition, double sleepers, double area, double cost);
// writes the values calculated and received by the program to the respective columns in the csv output file.

double sAccess(char access);
// calculates a price modifier based on wheterh there is adequate access to the build site

double localSite(char local);
// calculates a price modifier based on whether the site is local or not

double incrementCalc(double length, double start, double end);
// calculates the amount by which each bay's height increases over the previous bay using the known lengths and agles to calculate increments with trigonometry

double baySleepers(double length, double start, double increments, vector<double>&bayHeights);
// calculates the total number of sleepers required by the build and stores the individual bay totals in a vector

double wallCost(double wallArea, double localCost, double accessCost, double siteCost);
// calculates the total cost of the wall by multiplying all price modifiers and the total area of the wall 

double siteSurcharge(char siteDifficulty);
// calculates a price modifier based on the conditions of the work site

double areaCalc(double numSleepers); 
//calculates the area of the wall by using the known area of individual sleepers, and multiplying this with the total number of sleepers

int processFileInput(char userIn);

int main()
{
	// variable declarations
	char in, input, confirm, input2, localBuild, siteAccess, siteCond;
	double wallLength, numSleepers, startHeight, endHeight, totalCost, area, increment, local, accessSite, site;
	int jobNo = 1;
	ofstream outFile;
	ifstream inFile;
	string line;
	vector<double>bayHeights;

	// opening output file and providing column headings for future data entry
	outFile.open("retainingWalls.csv", ios::app);
	outFile << "ID, Length, Start Height, End Height, Local, Access, Site Condition, Number of Sleepers, Area, Cost, Sleeper Config\n";

	//opening input file 
	inFile.open("retainingWalls.csv");
	//display welcome message
	displayWelcome();

	// check the initial user input to determine whether there is a new file being created
	// or if there is an existing file to be read
	//if input is not valid, loop menu option until valid input is entered
	cin >> input;
	if (input == 'R' || input == 'r')
	{
		while (inFile.good())
		{
			getline(inFile, line);
			cout << line << endl;
		}
		cout << "Enter any key to quit  ";
		cin >> in;
		if (in)
		{
			inFile.close();
			outFile.close();
			exit(1);
		}
	}
	if (input != 'R' && input != 'r' && input != 'N' && input != 'n')
	{
		while (input != 'R' && input != 'r' && input != 'N' && input != 'n')
		{
			cout << "That was not a valid selection. Please enter a valid selection(N/R): ";
			cin >> input;
		}
	}
	

	// loop the main program each time the user opts for an additional quote at the end of the program
	do{
		cout << "This is job number " << jobNo << "." << endl;
		cout << "What is the length of the wall in metres?  ";
		cin >> wallLength;
		cout << "What is the start height of the wall in metres?  ";
		cin >> startHeight;
		cout << "What is the end height of the wall in metres?  ";
		cin >> endHeight;
		cout << "Is the wall to be built locally?  ";
		cin >> localBuild;
		local = localSite(localBuild);
		cout << "Is there adequate access to the site?  ";
		cin >> siteAccess;
		accessSite = sAccess(siteAccess);
		cout << "What is the difficulty of the site (N)ormal, (S)andy, (L)imestone, or (B)luestone (N/S/L/B)?";
		cin >> siteCond;
		site = siteSurcharge(siteCond);

		//The following functions perform the calculations to find the value by which the individual bay heights increase, the number of sleepers,
		//the area of the wall, and the cost of constructing the wall.
		increment = incrementCalc(wallLength, startHeight, endHeight);
		numSleepers = baySleepers(wallLength, startHeight, increment, bayHeights);
		area = areaCalc(numSleepers);
		totalCost = wallCost(area, local, accessSite, site);
		writeOut(outFile, jobNo, wallLength, startHeight, endHeight, localBuild, siteAccess, siteCond, numSleepers, area, totalCost);

		//print to screen the results of all calculations
		cout << "The wall will require " << numSleepers << " sleepers, will be " << area << "m2, and cost $" << totalCost << "." << endl;
		cout << "There will be " << wallLength / 2 << " bays in the following sleeper configuration: " << endl;

		//print the contents of bayHeights vector to screen and file, with values separated by |
		for (int a = 0; a < bayHeights.size(); a++){
			cout << bayHeights[a] << " | ";
			outFile << bayHeights[a] << "|";
		}
		cout << endl;
		outFile << endl;

		//prompt user for a confirmation of a second quote, otherwise program will terminate
		cout << "Would you like to begin a new quotation?(Y/N) ";
		cin >> confirm;
		bayHeights.clear();
		jobNo = jobNo ++;
		
	} while (confirm == 'y' || confirm == 'Y');
	cout << "Thankyou. Goodbye.";
	inFile.close();
	outFile.close();
	exit(1);
}


void displayWelcome()
{ // this is the welcome menu
	cout << "Welcome to the Quotation Assistant" << endl;  
	cout << "Use this application to generate the required number of sleepers for your job" << endl;
	cout << "and the initial quotation." << endl;
	cout << "==========================================================================" << endl;
	cout << "Would you like to generate a (N)ew quote or (R)etrieve an existing file (N/R)?" << endl;
}

double siteSurcharge(char siteDifficulty)
{
	//selection of price modifier based on user input
	//if input is not valid, loop menu option until valid input is entered
	double priceMod;
	if (siteDifficulty == 'N' || siteDifficulty == 'n')
	{
		priceMod = 1.0;
		return priceMod;
	}
	if (siteDifficulty == 'S' || siteDifficulty == 's')
	{
		priceMod = 1.10;
		return priceMod;
	}
	if (siteDifficulty == 'L' || siteDifficulty == 'l')
	{
		priceMod = 1.15;
		return priceMod;
	}
	if (siteDifficulty == 'B' || siteDifficulty == 'b')
	{
		priceMod = 1.30;
		return priceMod;
	}
	if (siteDifficulty != 'B' && 'b' && 'N'&& 'n' && 'S' && 's' && 'L' && 'l')
	{
		while (siteDifficulty != 'B'&& siteDifficulty != 'b' && siteDifficulty != 'L' && siteDifficulty != 'l' && siteDifficulty != 's' && siteDifficulty != 'S' && siteDifficulty != 'N' && siteDifficulty != 'n')
		{
			cout << "That was not a valid input. Please enter a valid input(N/S/L/B): ";
			cin >> siteDifficulty;
		}
	}
}
double incrementCalc(double length, double start, double end)
{
	//calculate the increment by which the bay height increases as the wall progresses
	//uses initial measurements and trigonometry 
	double angle, height, l, increment;
	height = end - start;
	l = length - 2.0;
	angle = atan(height / l);
	increment = 200 * tan(angle);
	return increment*0.01;
}

double baySleepers(double length, double start, double increments, vector<double>&bayHeights)
{
	//calculate the total number of sleepers by storing individual bay calculations in a vector
	//then return the sum of the individula bay sleeper totals
	int numBays = length / 2.0;
	double numSleepers, height, total;
	for (int i = 0; i < numBays; i++)
	{
		height = start + (i*(increments));
		numSleepers = round(height / 0.2);
		bayHeights.push_back(numSleepers);
		total += bayHeights[i];
	}
	return total;
}

double sAccess(char access)
{
	//selection of price modifier based on user input
	//if input is not valid, loop menu option until valid input is entered
	char userIn;
	double priceMod;
	if (access == 'Y' || access == 'y')
	{
		priceMod = 1.0;
		return priceMod;
	}
	if (access == 'N' || access == 'n')
	{
		priceMod = 1.30;
		return priceMod;
	}
	if (access != 'n' && 'N' && 'Y' && 'y')
	{
		while (access != 'n' && access != 'N' && access != 'Y' && access != 'y' )
		{
			cout << "That was not a valid input. Please enter a valid input(Y/N): ";
			cin >> access;
		}
	}
}
double localSite(char local)
{
	//selection of price modifier based on user input
	//if input is not valid, loop menu option until valid input is entered
	double priceMod;
	if (local == 'Y' || local == 'y')
	{
		priceMod = 1.0;
		return priceMod;
	}
	if (local == 'N' || local == 'n')
	{
		priceMod = 1.05;
		return priceMod;
	}
	if (local != 'n' && 'N' && 'Y' && 'y')
	{
		while (local != 'n' && local != 'N' && local != 'Y' && local != 'y')
		{
			cout << "That was not a valid input. Please enter a valid input(Y/N): ";
			cin >> local;
		}
	}
}
double areaCalc(double numSleepers)
{
	//calculate area of wall from sleeper totals and individual sleeper area
	double area = numSleepers * 0.4;
	return area;
}
double wallCost(double wallArea, double localCost, double accessCost, double siteCost)
{
	// calculate the total cost using price modifiers, area of wall and known unit price per sqare metre
	double totalCost, unitCost = 514.72, priceMod;
	priceMod = siteCost*localCost*accessCost;
	totalCost = (wallArea*unitCost)*priceMod;
	return totalCost;
}
void writeOut(ofstream& wallFile, int job, double length, double sHeight, double eHeight, char local, char access, char condition, double sleepers, double area, double cost)
{
	// write out data collected by program to separate columns in csv file
	wallFile << job << ",";
	wallFile << length << ",";
	wallFile << sHeight << ",";
	wallFile << eHeight << ",";
	wallFile << local << ",";
	wallFile << access << ",";
	wallFile << condition << ",";
	wallFile << sleepers << ",";
	wallFile << area << ",";
	wallFile << cost << ",";
}
