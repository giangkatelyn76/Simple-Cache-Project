// Katelyn Giang
// COSC 4310 - 01
// Dr. Jane Liu
// 5 November 2025

import java.util.LinkedList;
import java.util.Scanner;

public class SimpleCache {

    static class CacheBlock {
        int tag;
        boolean valid;

        public CacheBlock() {
            this.valid = false;
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter number of cache blocks: ");
        int numCacheBlocks = scanner.nextInt();

        System.out.print("Enter set associativity (1 for direct-mapped, 2 for 2-way, 4 for 4-way, 0 for fully associative): ");
        int associativity = scanner.nextInt();

        // Calculate number of sets
        int numSets = (associativity == 0) ? 1 : numCacheBlocks / associativity;
        int blocksPerSet = (associativity == 0) ? numCacheBlocks : associativity;

        CacheBlock[][] cache = new CacheBlock[numSets][blocksPerSet];
        for (int i = 0; i < numSets; i++) {
            for (int j = 0; j < blocksPerSet; j++) {
                cache[i][j] = new CacheBlock();
            }
        }

        // LRU lists for each set
        LinkedList<Integer>[] lruLists = new LinkedList[numSets];
        for (int i = 0; i < numSets; i++) {
            lruLists[i] = new LinkedList<>();
        }

        int totalReferences = 0;
        int missCount = 0;

        System.out.println("Enter block address references (enter -1 to stop):");
        while (true) {
            int blockAddress = scanner.nextInt();
            if (blockAddress == -1) {
                break;
            }

            totalReferences++;

            int setIndex;
            int tag;

            if (associativity == 0) { // Fully associative
                setIndex = 0;
                tag = blockAddress;
            } else { // Direct-mapped or Set-associative
                setIndex = blockAddress % numSets;
                tag = blockAddress / numSets;
            }

            boolean hit = false;
            int hitIndex = -1;

            // Check for hit
            for (int i = 0; i < blocksPerSet; i++) {
                if (cache[setIndex][i].valid && cache[setIndex][i].tag == tag) {
                    hit = true;
                    hitIndex = i;
                    break;
                }
            }

            if (hit) {
                // Update LRU: move to front
                lruLists[setIndex].remove((Integer) hitIndex);
                lruLists[setIndex].addFirst(hitIndex);
                System.out.println("Cache Hit for block address " + blockAddress);
            } else {
                missCount++;
                System.out.println("Cache Miss for block address " + blockAddress);

                // Find a block to replace (LRU)
                int replaceIndex;
                if (lruLists[setIndex].size() < blocksPerSet) { // Space available
                    replaceIndex = lruLists[setIndex].size(); // Use the next available slot
                } else { // LRU replacement
                    replaceIndex = lruLists[setIndex].removeLast();
                }

                // Update cache block
                cache[setIndex][replaceIndex].tag = tag;
                cache[setIndex][replaceIndex].valid = true;
                lruLists[setIndex].addFirst(replaceIndex);
            }

            // Print cache content
            System.out.println("Cache Content:");
            for (int i = 0; i < numSets; i++) {
                System.out.print("Set " + i + ": ");
                for (int j = 0; j < blocksPerSet; j++) {
                    if (cache[i][j].valid) {
                        System.out.print("[Tag: " + cache[i][j].tag + "] ");
                    } else {
                        System.out.print("[Empty] ");
                    }
                }
                System.out.println();
            }
            System.out.println("--------------------");
        }

        double missRate = (double) missCount / totalReferences * 100;
        System.out.printf("Cache Miss Rate: %.2f%%\n", missRate);

        scanner.close();
    }
}
