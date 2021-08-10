/**
 * @file graphics.cpp
 *
 * 画像描画関連のプログラムを集めたファイル．
 */

#include "graphics.hpp"
#include "font.hpp"

void RGBResv8BitPerColorPixelWriter::Write(Vector2D<int> pos, const PixelColor& c) {
    auto p = PixelAt(pos);
    p[0] = c.r;
    p[1] = c.g;
    p[2] = c.b;
}

void BGRResv8BitPerColorPixelWriter::Write(Vector2D<int> pos, const PixelColor& c) {
    auto p = PixelAt(pos);
    p[0] = c.b;
    p[1] = c.g;
    p[2] = c.r;
}

void DrawRectangle(PixelWriter& writer, const Vector2D<int>& pos,
                   const Vector2D<int>& size, const PixelColor& c) {
    for (int dx = 0; dx < size.x; ++dx) {
        writer.Write(pos + Vector2D<int>{dx, 0}, c);
        writer.Write(pos + Vector2D<int>{dx, size.y - 1}, c);
    }
    for (int dy = 1; dy < size.y -1; ++dy) {
        writer.Write(pos + Vector2D<int>{0, dy}, c);
        writer.Write(pos + Vector2D<int>{size.x - 1, dy}, c);
    }
}

void FillRectangle(PixelWriter& writer, const Vector2D<int>& pos,
                   const Vector2D<int>& size, const PixelColor& c) {
    for (int dy = 0; dy < size.y; ++dy) {
        for (int dx = 0; dx < size.x; ++dx) {
            writer.Write(pos + Vector2D<int>{dx, dy}, c);
        }
    }
}

// original mark
void draw_danbo_mark(int x_first, int y_first, PixelWriter& pixel_writer){
    int x_last = x_first + 160-1;
    int y_last = y_first + 100-1;
    for (int x = x_first; x <= x_last; ++x) {
        for (int y = y_first; y <= y_last; ++y){
            pixel_writer.Write(Vector2D<int>{x, y}, {240,210,150});
            if(y == y_first) pixel_writer.Write(Vector2D<int>{x, y}, {0,0,0});
            else if(y == y_last) pixel_writer.Write(Vector2D<int>{x, y}, {0,0,0});
            else if(x == x_first) pixel_writer.Write(Vector2D<int>{x, y}, {0,0,0});
            else if(x == x_last) pixel_writer.Write(Vector2D<int>{x, y}, {0,0,0});
        }
    }
    int x_half = (x_last - x_first + 1) / 2 + x_first;
    int y_half = (y_last - y_first + 1) / 2 + y_first;
    // right eye
    WriteAscii(pixel_writer, Vector2D<int>{x_half - 40, y_half - 20}, (unsigned char)0xfc, {0,0,0});
    WriteAscii(pixel_writer, Vector2D<int>{x_half - 32, y_half - 20}, (unsigned char)0xfd, {0,0,0});
    // left eye
    WriteAscii(pixel_writer, Vector2D<int>{x_half + 24, y_half - 20}, (unsigned char)0xfc, {0,0,0});
    WriteAscii(pixel_writer, Vector2D<int>{x_half + 32, y_half - 20}, (unsigned char)0xfd, {0,0,0});
    // mouth
    WriteAscii(pixel_writer, Vector2D<int>{x_half-8, y_half + 15}, (unsigned char)0xfe, {0,0,0});
    WriteAscii(pixel_writer, Vector2D<int>{x_half  , y_half + 15}, (unsigned char)0xff, {0,0,0});
    // Logo
    WriteString(pixel_writer, Vector2D<int>{x_first+3, y_first+2}, "Danbo", {50, 0, 0});
}
// original mark

void DrawDesktop(PixelWriter& writer) {
    const auto width = writer.Width();
    const auto height = writer.Height();
    FillRectangle(writer,
                  {0, 0},
                  {width, height - 50},
                  kDesktopBGColor);
    FillRectangle(writer,
                  {0, height - 50},
                  {width, 50},
                  {1, 8, 7});
    FillRectangle(writer,
                  {0, height - 50},
                  {width / 5, 50},
                  {80, 80, 80});
    DrawRectangle(writer,
                  {10, height - 40},
                  {30, 30},
                  {160, 160, 160});

}


void DrawDesktop2(PixelWriter& writer) {
    const auto width = writer.Width();
    const auto height = writer.Height();
    FillRectangle(writer,
                  {0, 0},
                  {width, height - 50},
                  kDesktopBGColor);
    FillRectangle(writer,
                  {0, height - 50},
                  {width, 50},
                  {1, 8, 7});
    FillRectangle(writer,
                  {0, height - 50},
                  {width / 5, 50},
                  {255, 255, 255});
    DrawRectangle(writer,
                  {10, height - 40},
                  {30, 30},
                  {255, 255, 0});
}

void DrawImage(PixelWriter& writer, std::vector< std::vector<PixelColor> > colors) {
    const auto x = writer.Width();
    const auto y = writer.Height();
    //const auto x = 10;//426;
    //const auto y = 10;//237;
    for (int dy = 0; dy < y; ++dy) {
        for (int dx = 0; dx < x; ++dx) {
            // print
            writer.Write( Vector2D<int>{dx, dy}, colors[dy][dx] );
        }
    }
}