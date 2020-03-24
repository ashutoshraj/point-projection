import numpy as np
import matplotlib.pyplot as plt


class Plotter(object):
    def __init__(self, np_arr, plane, threshold=1, visualisation=False, location=False):
        self.np_arr = np_arr
        self.x = np_arr[:, 0]
        self.y = np_arr[:, 1]
        self.z = np_arr[:, 2]
        self.threshold = threshold
        self.plane = plane
        self.visualisation = visualisation
        self.location = location

    def plot(self):
        xedges, yedges, zedges = self.configuration(self.x, self.y, self.z)

        H, edges = np.histogramdd(self.np_arr, bins=(xedges, yedges, zedges))

        if self.plane.lower().replace(' ', '') == 'xy' or self.plane.lower().replace(' ', '') == 'yx':
            return self.xy(H, xedges, yedges, self.visualisation)
        elif self.plane.lower().replace(' ', '') == 'zx' or self.plane.lower().replace(' ', '') == 'xz':
            return self.xz(H, xedges, zedges, self.visualisation)
        elif self.plane.lower().replace(' ', '') == 'yz' or self.plane.lower().replace(' ', '') == 'zy':
            return self.yz(H, yedges, zedges, self.visualisation)

    def configuration(self, x_vals, y_vals, z_vals):
        xmax, ymax, zmax = int(x_vals.max()) + 1, int(y_vals.max()) + 1, int(z_vals.max()) + 1
        xmin, ymin, zmin = int(x_vals.min()), int(y_vals.min()), int(z_vals.min())
        xrange, yrange, zrange = xmax - xmin, ymax - ymin, zmax - zmin
        xedges = np.linspace(xmin, xmax, (xrange + 1), dtype=int)
        yedges = np.linspace(ymin, ymax, (yrange + 1), dtype=int)
        zedges = np.linspace(zmin, zmax, (zrange + 1), dtype=int)
        return xedges, yedges, zedges

    def plotting(self, sum_of_all_z, xedges, yedges, x_vals, y_vals, labelx, labely):
        plt.imshow(sum_of_all_z.transpose(), extent=[xedges[0], xedges[-1], yedges[0], yedges[-1]],
                   interpolation='none',
                   origin='low')
        plt.grid(b=True, which='both')
        plt.xticks(xedges)
        plt.yticks(yedges)
        plt.xlabel(labelx)
        plt.ylabel(labely)
        plt.plot(x_vals, y_vals, 'ro')
        plt.show()

    def xy(self, H, xedges, yedges, visual):
        """
        For histogram of points in xy plane.
        """
        _sum = np.sum(H, axis=2)
        loc = np.argwhere(_sum > self.threshold)

        if visual:
            self.plotting(_sum, xedges, yedges, self.x, self.y, 'X-AXIS', 'Y-AXIS')

        arr = []
        for i in loc:
            arr.append(_sum[i[0], i[1]])
        arr = np.vstack(arr)
        arr = arr / np.linalg.norm(arr)
        loc[:, 0] += int(min(self.x))
        loc[:, 1] += int(min(self.y))
        if self.location:
            return loc
        else:
            return np.concatenate((loc, arr), axis=1)

    def xz(self, H, xedges, zedges, visual):
        """
        For histogram of points in zx plane.
        """
        _sum = np.sum(H, axis=1)
        loc = np.argwhere(_sum > self.threshold)

        if visual:
            self.plotting(_sum, xedges, zedges, self.x, self.z, 'X-AXIS', 'Z-AXIS')

        arr = []
        for i in loc:
            arr.append(_sum[i[0], i[1]])
        arr = np.vstack(arr)
        arr = arr / np.linalg.norm(arr)
        loc[:, 0] += int(min(self.x))
        loc[:, 1] += int(min(self.z))
        if self.location:
            return loc
        else:
            return np.concatenate((loc, arr), axis=1)

    def yz(self, H, yedges, zedges, visual):
        """
        For histogram of points in zx plane.
        """
        _sum = np.sum(H, axis=0)
        loc = np.argwhere(_sum > self.threshold)

        if visual:
            self.plotting(_sum, yedges, zedges, self.y, self.z, 'Y-AXIS', 'Z-AXIS')

        arr = []
        for i in loc:
            arr.append(_sum[i[0], i[1]])
        arr = np.vstack(arr)
        arr = arr / np.linalg.norm(arr)
        loc[:, 0] += int(min(self.y))
        loc[:, 1] += int(min(self.z))

        if self.location:
            return loc
        else:
            return np.concatenate((loc, arr), axis=1)


if __name__ == '__main__':
     arr = np.load('sample.npy')
     a = Plotter(arr, 'zx', 0, visualisation=True).plot()
     b = Plotter(arr, 'zx', 0, False).plot()
     c = Plotter(arr, 'yz', 0, False).plot()
