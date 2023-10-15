import turtle
from PIL import Image


class ITP:
    def __init__(self, im):
        self.hz = turtle.Turtle()
        turtle.colormode(255)
        self.hz.speed(0)
        self.hz.ht()
        turtle.tracer(0, 0)
        # turtle.tracer(0, 0) used with turtle.update() to speed up turtle drawing process.

        if (im.width % 2) != 0:
            self.width = im.width - 1
        else:
            self.width = im.width

        if (im.height % 2) != 0:
            self.height = im.height - 1
        else:
            self.height = im.height
        # Ensures that image height and width are even, to avoid problems with effect functions.

        self.x_increase = 0
        self.y_increase = 0
        self.hz.penup()
        self.hz.goto(-self.width / 2, -self.y_increase + self.height / 2)
        # Makes turtle go to upper-left coordinates of the image, while ensuring that turtle drawing is centered.
        self.hz.pendown()
        self.rgb_im = im.convert('RGB')

    def process_complete(self):
        turtle.setup(self.width, self.height)
        turtle.screensize(self.width, self.height)
        # Makes python turtle graphics the same size as the image
        turtle.update()
        print("Finished!")
        turtle.exitonclick()

    def reset(self):
        # Resets starting position and coord-increase variables.
        self.x_increase = 0
        self.y_increase = 0
        self.hz.penup()
        self.hz.goto(-self.width / 2, self.y_increase + self.height / 2)
        self.hz.pendown()

    def rgb_conversion(self): 
        count = -1
        # count determines every 2 pixels width wise
        index = 0
        # index determines position of pixel in a 2x2 box. When index equals:
        # 0:Top-left, 1:Bottom-left, 2:Top-right, 3:Bottom-right
        for i in range(self.width*self.height):
            if index == 4:
                index = 0
                
            if index == 0:
                count += 1
                if count == self.width / 2:
                    self.y_increase += 2
                    count = 0
                    # Resets count when moving to the next row. (Down 2 pixels)
                    self.hz.penup()
                    self.hz.goto(-self.width/2, -self.y_increase + self.height/2)
                    # Positions turtle to the very left and down, depending on what row it is on.
                    self.hz.pendown()
                    print(f'Loading... {round(i/(self.width*self.height) * 100, 2)}%')

                self.x_increase = 2 * count

                x = 0 + self.x_increase
                y = 0 + self.y_increase
            elif index == 1:
                x = 0 + self.x_increase
                y = 1 + self.y_increase
            elif index == 2:
                x = 1 + self.x_increase
                y = 0 + self.y_increase
            elif index == 3:
                x = 1 + self.x_increase
                y = 1 + self.y_increase

            index += 1
            yield self.rgb_im.getpixel((x, y))

    def blurred(self):
        # blurred() places every 2x2 pixels from order of:
        #       top-left, bottom-left, top-right, bottom-right
        # into one row
        self.reset()
        self.hz.width(2)
        # Has width of 2 to cover every 2 rows, which had been "mushed" into 1 row

        for rgb in self.rgb_conversion():    
            r, g, b = rgb
            c = (r, g, b)
            self.hz.color(c)
            self.hz.forward(0.5)
            # Moves forward by 0.5 pixels since 4 pixels are "mushed" into 2 pixels
            # print(f"i:{i} index:{index} rgb:{r},{g},{b} count:{count} (x,y):({x},{y})")

        self.process_complete()

    def hd(self):
        # hd() makes a high definition image onto Python w/ Turtle
        self.reset()
        self.hz.width(1)

        count = -1
        for i in range(self.width*self.height):
            count += 1
            
            if count == self.width:
                self.y_increase += 1
                count = 0
                self.hz.penup()
                self.hz.goto(-self.width / 2, -self.y_increase + self.height / 2)
                self.hz.pendown()
                print(f'Loading... {round(self.y_increase / self.height * 100, 2)}%')
                self.x_increase = count
                
            x = self.x_increase
            y = self.y_increase
            r, g, b = self.rgb_im.getpixel((x, y))
            c = (r, g, b)
            self.hz.color(c)
            self.hz.forward(1)
            self.x_increase += 1
            
        self.process_complete()

    def pixelated(self, n):
        # n represents every n by n pixels that will become "pixelated"
        # In other words, if n is 2, then every 2x2 pixels' rgb values will be averaged and turtle will
        # "replace" those 2x2 pixels with the average rgb value.

        self.hz.width(n)
        mix = 0
        index = 0
        # index determines position of pixel in a n by n box, by column down, left to right. Ex.:
        #   if n is 2, then the pixel position in a 2x2 box based on index is: (In the order of index:position)
        #       0:Top-left, 1:Bottom-left, 2:Top-right, 3:Bottom-left

        x_start = 0
        # x_start is the x-coord depending on what column of an n by n box the index is on.
        y_start = 0
        # y_start is the y-coord depending on what row of an n by n box the index is on.

        while (self.width % n) != 0:
            self.width -= 1
        while (self.height % n) != 0:
            self.height -= 1
        # Makes sures the dimensions of the image are divisible by n

        self.reset()

        rgb4r = []
        rgb4g = []
        rgb4b = []
        # Stores the rgb values of the pixels, in order of index, in an n by n box.
        # color of each pixel is: (rgb4r[index], rgb4g[index], rgb4b[index])
        for i in range(n * n):
            rgb4r.append(0)
            rgb4g.append(0)
            rgb4b.append(0)

        count = -1
        # In pixelated(), count determines every n pixels width wise

        for i in range(self.width*self.height):
            if y_start == n:
                y_start = 0

            if (index % n) == 0 and index != 0:
                # Checks if index equals the multiple of n, excluding 0
                x_start += 1

            if index == n * n:
                index = 0
            if x_start == n:
                x_start = 0

            if index == 0:
                count += 1
                if count == self.width / n:
                    self.y_increase += n
                    count = 0
                    # Resets count when moving to the next row. (Down n pixels)
                    self.hz.penup()
                    self.hz.goto(-self.width / 2, -self.y_increase + self.height / 2)
                    # Positions turtle to the very left and down, depending on what row it is on.
                    self.hz.pendown()
                    print(f'Loading... {round(i / (self.width * self.height) * 100, 2)}%')

                self.x_increase = n * count
                # x_increase increases by n each time count increases by 1
                # Retrieves the next n*n pixels in an n by n box.

            x = x_start + self.x_increase
            y = y_start + self.y_increase

            y_start += 1
            index += 1
            r, g, b = self.rgb_im.getpixel((x, y))
            # Retrieves rgb values of the pixel at (x,y)

            mix_to_index = mix + 1
            if mix_to_index == n * n:
                mix_to_index = 0

            rgb4r[mix_to_index] = r
            rgb4g[mix_to_index] = g
            rgb4b[mix_to_index] = b
            # Appends rgb values into their respective lists for every 4 pixels

            mix += 1
            if mix == n * n:
                # Once n*n pixels' rgb values are retrieved, the average of the pixels' color values is calculated.
                r_mix = sum(rgb4r) // (n * n)
                g_mix = sum(rgb4g) // (n * n)
                b_mix = sum(rgb4b) // (n * n)
                c = (r_mix, g_mix, b_mix)
                mix = 0
                self.hz.color(c)
                self.hz.forward(n)
        self.process_complete()


res = True

picture = input("Enter the source of the image you want to draw:\n")
while res:
    try:
        image = ITP(Image.open(f'{picture}'))
        break
    except FileNotFoundError:
        print("Sorry, the file name you entered was not found. \n"
              "This is case-sensitive and make sure the .png or .jpg is included. Please run the program again.")
        exit()

effect = input("Enter one of the effects listed below for how you want your image to turn out:\n"
               "Note: Type the effects exactly as listed.\n"
               "blurred\n"
               "hd\n"
               "pixelated\n")

while res:
    if effect == "pixelated":
        k = int(input("Choose a 'degree' of pixelation. Ex. If 2 is chosen, then every 2x2 pixels will be pixelated.\n"
                      "MUST be a whole number. Recommended 11 or below.\n"))
        image.pixelated(k)
    else:
        try:
            exec(f'image.{effect}()')
            break
        except AttributeError:
            print("The effect you typed is invalid! Make sure you type one of the effects listed.\n"
                  "Please run the program again.")
            exit()
