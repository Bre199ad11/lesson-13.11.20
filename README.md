#include <iostream>
#include <fstream>

using namespace std;

double det_rec(double** a, int n) {
	if (n == 1) return a[0][0];
	double d = 0;
	for (int j = 0; j < n; j++) {
		double** m = new double*[n - 1];
		for (int i = 0; i < n - 1; i++) {
			m[i] = new double[n - 1];
		}
		for (int i = 0; i < n - 1; i++) {
			for (int k = 0; k < n - 1; k++) {
				m[i][k] = a[i + 1][k + (k >= j)];
			}
		}
		d += a[0][j] * det_rec(m, n - 1) * (j % 2 == 0 ? 1 : -1);
		for (int i = 0; i < n - 1; i++) {
			delete[] m[i];
		}
		delete[] m;
	}
	return d;
}

double det_iter(double** a, int n) {
	double** t = new double*[n];
	for (int i = 0; i < n; i++) {
		t[i] = new double[n];
		for (int j = 0; j < n; j++) {
			t[i][j] = a[i][j];
		}
	}
	double d = 1;
	for (int i = 0; i < n; i++) {
		int c = i;
		while (c < n && abs(t[c][i]) < 1e-15) c++;
		if (c == n) return 0;
		swap(t[c], t[i]);
		for (int j = i + 1; j < n; j++)
			t[i][j] /= t[i][i];
		d *= t[i][i];
		t[i][i] = 1;
		for (int k = i + 1; k < n; k++) {
			double y = t[k][i];
			for (int j = 0; j < n; j++) {
				t[k][j] -= t[i][j] * y;
			}
		}
	}
	for (int i = 0; i < n; i++) {
		delete[] t[i];
	}
	delete[] t;
	return d;
}

int main() {
	ifstream fin("input.txt");
	int n;
	fin >> n;
	double** a = new double*[n];
	for (int i = 0; i < n; i++) {
		a[i] = new double[n];
		for (int j = 0; j < n; j++) {
			fin >> a[i][j];
		}
	}
	cout << det_rec(a, n) << " " << det_iter(a, n);
}
