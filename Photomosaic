###########################################################################g
# FILE : ex6.py
# WRITER : Hagit_Bachar , hagitba , 208349746
# EXERCISE : intro2cs ex6 2016-2017
# DESCRIPTION: This program make a photo mosaic by some function, the way that
#  she do that is to filter a list of many tiles in order to create a smaller
#  list of best tiles, (that their average color is very close to the average
#  color of the objective).
# The program uses 'get best tiles' function to do this.
# After that the program uses 'choose tile' function that choose the best
#  tile, that will fit the picture, by compare pixels.
# The function 'get piece' helps the program to divide the real image to
# tiles, and 'set piece' function changes each tile of the real image, and by
# 'choose tile' function choose the best tile to replace him.
#############################################################################

import mosaic
import sys
import copy


def compare_pixel(pixel1, pixel2):
    """
    This function compares between 2 pixels, and returns the sum of the
    distance between the pixels
    """
    abs_value = (abs(pixel1[0] - pixel2[0]) + abs(pixel1[1] - pixel2[1])
                 + abs(pixel1[2] - pixel2[2]))
    return abs_value


def compare(image1, image2):
    """
    This function compares between 2 images by Compare Pixel function,
    and returns the sum of all the pixels of the images
    """
    height_image1 = len(image1)
    width_image1 = len(image1[0])
    height_image2 = len(image2)
    width_image2 = len(image2[0])
    range_height = min(height_image1, height_image2)
    range_width = min(width_image1, width_image2)
    the_sum = 0

    for i in range(range_height):
        for j in range(range_width):
            absolute = compare_pixel(image1[i][j], image2[i][j])
            the_sum = the_sum + absolute

    return the_sum


def get_piece(image, upper_left, size):
    """
    This function returns a piece of the image, by the params 'upper_left'
    and 'size'
    """
    column_list = []
    start_column = upper_left[0]
    start_row = upper_left[1]
    end_column = min(start_column + size[0], len(image))
    end_row = min(start_row + size[1], len(image[0]))

    for k in range(start_column, end_column):
        row_list = []

        for l in range(start_row, end_row):
            row_list.append(image[k][l])
        column_list.append(row_list)

    return column_list


def set_piece(image, upper_left, piece):
    """
    This function changes a piece in the image
    """
    range1 = min(len(piece), (len(image) - upper_left[0]))
    range2 = min(len(piece[0]), (len(image[0]) - upper_left[1]))
    for m in range(range1):
        for n in range(range2):
            image[m + upper_left[0]][n + upper_left[1]] = piece[m][n]


def average(image):
    """
    This function returns the average color of the pixels in the image
    """
    red_sum = 0
    green_sum = 0
    blue_sum = 0
    average_red = 0
    average_green = 0
    average_blue = 0
    divisor = len(image[0]) * len(image)

    for i in range(len(image)):
        for j in range(len(image[i])):
            red_sum = red_sum + image[i][j][0]
            green_sum = green_sum + image[i][j][1]
            blue_sum = blue_sum + image[i][j][2]

    if divisor != 0:
        average_red = red_sum / divisor
        average_green = green_sum / divisor
        average_blue = blue_sum / divisor

    return (average_red, average_green, average_blue)


def preprocess_tiles(tiles):
    """
    This function returns a list of average color of each tile
    """
    list_tiles = []
    for image in tiles:
        average_image = average(image)
        list_tiles.append(average_image)

    return list_tiles


def get_best_tiles(objective, tiles, averages, num_candidates):
    """
    This function returns a list of tiles that their average color is
    very close to the average color of the objective
    """
    the_list_tiles = []
    average_objective = average(objective)
    list_sum_of_distance = []
    BIGGER_NUMBER = 767

    for i in range(len(averages)):
        sum_of_distance = compare_pixel(averages[i], average_objective)
        list_sum_of_distance.append(sum_of_distance)

    while len(the_list_tiles) < num_candidates:
        # The function returns a list that her length is 'num candidates'
        index = list_sum_of_distance.index(min(list_sum_of_distance))
        the_list_tiles.append(tiles[index])
        list_sum_of_distance[index] = BIGGER_NUMBER

    return the_list_tiles


def choose_tile(piece, tiles):
    """
    This function chooses the best tile from a list of tiles
    """
    compare_list = []
    for i in range(len(tiles)):
        distance = compare(piece, tiles[i])
        compare_list.append(distance)

    index_tile = compare_list.index(min(compare_list))

    return tiles[index_tile]


def make_mosaic(image, tiles, num_candidates):
    """
    This function make a photo mosaic by the previous functions
    """
    the_image = copy.deepcopy(image)
    averages = preprocess_tiles(tiles)
    num_candidates = int(num_candidates)
    height_tile = len(tiles[0])
    width_tile = len(tiles[0][0])

    for i in range(0, len(image), height_tile):
        for j in range(0, len(image[0]), width_tile):
            # The function run over the image and replace every tile by
            # 'get piece' and 'set piece' functions

            piece = get_piece(the_image, (i, j), (height_tile, width_tile))

            the_tiles = get_best_tiles(piece, tiles, averages,
                                       num_candidates)
            one_tile = choose_tile(piece, the_tiles)
            set_piece(the_image, (i, j), one_tile)

    return the_image


def main():
    """
    This function make the program run
    """

    tiles_dir = sys.argv[2]
    tile_height = int(sys.argv[4])
    tiles = mosaic.build_tile_base(tiles_dir, tile_height)

    image_filename = sys.argv[1]
    image = mosaic.load_image(image_filename)

    num_candidates = sys.argv[5]
    objective = make_mosaic(image, tiles, num_candidates)

    filename = sys.argv[3]
    mosaic.save(objective, filename)


if __name__ == "__main__":
    WRONG_PARAM_MSG = "Wrong number of parameters. The correct usage is:\n" \
                      "ex6.py <image_source> <images_dir> <output_name>" \
                      " <tile_height> <num_candidates>"
    numbers_param = 6
    if len(sys.argv) == numbers_param:
        main()

    else:
        print(WRONG_PARAM_MSG)
