#include <iostream>
using namespace std;

const int MAX_N = 100; // حداکثر تعداد نمونه
const int MAX_M = 10; // حداکثر تعداد ویژگی

// تابع رگرسیون خطی چندمتغیره
void linearRegression(double X[MAX_N][MAX_M], double Y[MAX_N], int n, int m, double weights[MAX_M + 1]) {
    double x_mean[MAX_M] = {0};
    double y_mean = 0;

    // میانگین‌ها
    for (int j = 0; j < m; j++) {
        for (int i = 0; i < n; i++) {
            x_mean[j] += X[i][j];
        }
        x_mean[j] /= n;
    }
    for (int i = 0; i < n; i++) {
        y_mean += Y[i];
    }
    y_mean /= n;

    // محاسبه وزن‌ها
    for (int j = 0; j < m; j++) {
        double numerator = 0, denominator = 0;
        for (int i = 0; i < n; i++) {
            numerator += (X[i][j] - x_mean[j]) * (Y[i] - y_mean);
            denominator += (X[i][j] - x_mean[j]) * (X[i][j] - x_mean[j]);
        }

        if (denominator != 0)
            weights[j] = numerator / denominator;
        else {
            weights[j] = 0;
            cout << "هشدار: ویژگی x" << j + 1 << " مقدار ثابت دارد، وزن آن صفر در نظر گرفته شد." << endl;
        }
    }

    // محاسبه bias
    double bias = y_mean;
    for (int j = 0; j < m; j++) {
        bias -= weights[j] * x_mean[j];
    }
    weights[m] = bias;
}

int main() {
    int n, m;
    cout << "تعداد نمونه‌ها (حداکثر " << MAX_N << "): ";
    cin >> n;
    if (n > MAX_N || n <= 0) {
        cout << "تعداد نمونه‌ها نامعتبر است." << endl;
        return 1;
    }

    cout << "تعداد ویژگی‌ها (حداکثر " << MAX_M << "): ";
    cin >> m;
    if (m > MAX_M || m <= 0) {
        cout << "تعداد ویژگی‌ها نامعتبر است." << endl;
        return 1;
    }

    double X[MAX_N][MAX_M];
    double Y[MAX_N];

    cout << "\nوارد کردن داده‌ها (ویژگی‌ها و خروجی):" << endl;
    for (int i = 0; i < n; i++) {
        cout << "نمونه " << i + 1 << ":" << endl;
        for (int j = 0; j < m; j++) {
            cout << " x" << j + 1 << ": ";
            cin >> X[i][j];
        }
        cout << " y: ";
        cin >> Y[i];
    }

    double weights[MAX_M + 1];
    linearRegression(X, Y, n, m, weights);

    // چاپ مدل
    cout << "\nمدل به‌دست‌آمده: y = ";
    for (int j = 0; j < m; j++) {
        cout << weights[j] << "*x" << j + 1;
        if (j < m - 1) cout << " + ";
    }
    cout << " + " << weights[m] << endl;

    // پیش‌بینی
    double testX[MAX_M];
    cout << "\nبرای پیش‌بینی y جدید، مقدار ویژگی‌ها را وارد کنید:" << endl;
    for (int j = 0; j < m; j++) {
        cout << "x" << j + 1 << ": ";
        cin >> testX[j];
    }

    double prediction = weights[m];
    for (int j = 0; j < m; j++) {
        prediction += weights[j] * testX[j];
    }
    cout << "y پیش‌بینی شده: " << prediction << endl;

    return 0;
}
