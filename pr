/** 
 * Class representing an efficient implementation of a 2-dimensional table 
 * when lots of repeated entries as a doubly linked list. Idea is to record entry only when a 
 * value changes in the table as scan from left to right through 
 * successive rows.
 * 
 * @author Jerry Onyango
 * @param <E> type of value stored in the table
 */
package compression;

class CompressedTable<E> implements TwoDTable<E> {
	// List holding table entries - do not change
	// We've made the variables protected to facilitate testing (grading)
	protected CurDoublyLinkedList<Association<RowOrderedPosn, E>> tableInfo;
	protected int numRows, numCols; // Number of rows and cols in table

	/**
	 * Constructor for table of size rows x cols, all of whose values are initially
	 * set to defaultValue
	 * 
	 * @param rows
	 *            # of rows in table
	 * @param cols
	 *            # of columns in table
	 * @param defaultValue
	 *            initial value of all entries in table
	 */
	public CompressedTable(int rows, int cols, E defaultValue) {
		//TODO: fix based on specifications
		numRows = rows;
		numCols = cols;
		this.tableInfo = new CurDoublyLinkedList<Association<RowOrderedPosn, E>>();

		RowOrderedPosn firstPos = new RowOrderedPosn(0, 0, rows, cols);
		Association<RowOrderedPosn, E> firstElem = new Association<RowOrderedPosn, E>(firstPos, defaultValue);
		tableInfo.add(firstElem);
	}

	/**
	 * Given a (x, y, rows, cols) RowOrderedPosn object, it searches for it in the
	 * table which is represented as a doubly linked list with a current pointer. If
	 * the table contains the (x,y) cell, it sets the current pointer to it.
	 * Otherwise it sets it to the closest cell in the table which comes before that
	 * entry.
	 * 
	 * e.g., if the table only contains a cell at (0,0) and you pass the cell (3,3)
	 * it will set the current to (0,0).
	 */
	private void find(RowOrderedPosn findPos) {
		tableInfo.first();
		Association<RowOrderedPosn, E> entry = tableInfo.currentValue();
		RowOrderedPosn pos = entry.getKey();
		while (!findPos.less(pos)) {
			// search through list until pass elt looking for
			tableInfo.next();
			if (tableInfo.isOff()) {
				break;
			}
			entry = tableInfo.currentValue();
			pos = entry.getKey();
		}
		tableInfo.back(); // Since passed desired entry, go back to it.
	}

	/**
	 * Given a legal (row, col) cell in the table, update its value to newInfo. 
	 * 
	 * @param row
	 *            row of cell to be updated
	 * @param col
	 *            column of cell to be update
	 * @param newInfo
	 *            new value to place in cell (row, col)
	 */
	public void updateInfo(int row, int col, E newInfo) {
		//TODO: fix
		if (row>=this.numRows || col >= this.numCols){ // if greater than row or colum
			throw new IndexOutOfBoundsException("position not in range");
		}
		RowOrderedPosn newPos = new RowOrderedPosn(row, col, numRows, numCols); // variable for new position
		find(newPos);

		boolean isNewPosNode = tableInfo.currentValue().getKey().equals(newPos); 

		RowOrderedPosn nextPos = newPos.next(); //variable foe the next position
		boolean nextposExists = nextPos != null; // checking for the next position

		Association<RowOrderedPosn, E> entry = tableInfo.currentValue();
		E currentInfo = entry.getValue(); // geting the value of the current position

		if (!currentInfo.equals(newInfo)){// if the new vaule is similar to the current, nothing happens
			// return;
		if (newPos.equals(entry.getKey())){
			entry.setValue(newInfo);
		} else {
			tableInfo.addAfterCurrent(new Association<RowOrderedPosn, E>(newPos, newInfo)); // =======
		}

		if (nextposExists){
			tableInfo.next(); // move to the next space if possible
			boolean isNextPosNode = !tableInfo.isOffRight() && tableInfo.currentValue().getKey().equals(nextPos);
			tableInfo.back();

			if (isNewPosNode && isNextPosNode){
				tableInfo.next();
				boolean isNextPosNodeSameInfo = tableInfo.currentValue().getValue().equals(newInfo);
				tableInfo.back();

				if(isNextPosNodeSameInfo){ // if next position is similar to the previous
					tableInfo.removeCurrent(); // removes the current position
					tableInfo.removeCurrent();
					tableInfo.back(); 
					tableInfo.addAfterCurrent(new Association<RowOrderedPosn, E>(nextPos, newInfo));
				}else{ // if next position is not the same as previous
					tableInfo.removeCurrent();
					tableInfo.addAfterCurrent(new Association<RowOrderedPosn, E>(newPos, newInfo)); // ----------------------------------------
				}
				
			}else if(isNewPosNode && !isNextPosNode){ // if this is a new position and this is not the next position
				// how about we create another node first, then add
				
				if (row ==0 && col == 0){
					tableInfo.addAfterCurrent(new Association<RowOrderedPosn, E>(newPos, newInfo));
					tableInfo.back();
					tableInfo.removeCurrent();
				}else{
					tableInfo.removeCurrent();
				tableInfo.back(); //if offright then we cannot go back --------------------------
				tableInfo.addAfterCurrent(new Association<RowOrderedPosn, E>(nextPos, newInfo));
				tableInfo.addAfterCurrent(new Association<RowOrderedPosn, E>(nextPos, currentInfo));
				tableInfo.back(); // -----------------------------------------------

				}

			}else if(!isNewPosNode && isNextPosNode){ // if this is not a new position and this is the next position

				tableInfo.next();
				boolean isNextPosNodeSameInfo = tableInfo.currentValue().getValue().equals(newInfo);
				tableInfo.back();

				if (isNextPosNodeSameInfo){ // if next node has same info as previous
					tableInfo.next();
					tableInfo.removeCurrent();
					tableInfo.back();
					tableInfo.addAfterCurrent(new Association<RowOrderedPosn, E>(newPos, currentInfo));
				}else{
					tableInfo.addAfterCurrent(new Association<RowOrderedPosn, E>(newPos, currentInfo));
				}

			}else if(!isNewPosNode && !isNextPosNode){
				//tableInfo.addAfterCurrent(new Association<RowOrderedPosn, E>(newPos, currentInfo)); -------------------------
					tableInfo.addAfterCurrent(new Association<RowOrderedPosn, E>(newPos, newInfo)); // 140 just updated
					tableInfo.back(); // messed up this part---------------================
				
			}
		}else{
			if (isNewPosNode){ // if this is a new position
				tableInfo.back(); // we first move back
				tableInfo.addAfterCurrent(new Association<RowOrderedPosn, E>(newPos, currentInfo)); // add our node
				tableInfo.next(); // we go  next
				tableInfo.removeCurrent(); // remove current
			 }
		}
		tableInfo.back();
		if (tableInfo.isOff() || !tableInfo.currentValue().getValue().equals(newInfo)){
			return;
			// if current is either offright or offleft or the new value is not equal to the current value - nothing happens
		}else{ // if table is not off and current value is equal to new value
			tableInfo.next();
			tableInfo.removeCurrent();
			tableInfo.back();
			return;
		}

		} 
		else {
			return;
		}

	}

	/**
	 * Returns contents of specified cell
	 * 
	 * @pre: (row,col) is legal cell in table
	 * 
	 * @param row
	 *            row of cell to be queried
	 * @param col
	 *            column of cell to be queried
	 * @return value stored in (row, col) cell of table
	 */
	public E getInfo(int row, int col) {
		//if (tableInfo.RowOrderedPosn(row,col,numRows,numCols)){
			//return 
		if (row >=numRows || col>=numCols){
			throw new IndexOutOfBoundsException("Not a legal cell in the table");
		}
		find(new RowOrderedPosn(row, col, this.numRows, this.numCols));
		return tableInfo.currentValue().getValue();   // fix this!
	}

	/**
	 *  @return
	 *  		 succinct description of contents of table
	 */
	public String toString() { // do not change
	    return tableInfo.otherString();
	}

	public String entireTable() { //do not change
		StringBuilder ans = new StringBuilder("");
		for (int r = 0; r<numRows; r++) {
			for (int c = 0; c< numCols; c++) {
				ans.append(this.getInfo(r, c));
			}
			ans.append("\n");
		}
		return ans.toString();

	}

	/**
	 * program to test implementation of CompressedTable
	 * @param args
	 * 			ignored, as not used in main
	 */
	public static void main(String[] args) {
		
		// add your own tests to make sure your implementation is correct!!
		CompressedTable<String> table = new CompressedTable<String>(5, 6, "x");

		System.out.println("table is " + table);

		table.updateInfo(0, 0, "b");
		//System.out.println("table is " + table);

		table.updateInfo(0, 1, "a");
		//System.out.println("table is " + table);

		System.out.println("------------------------------------------------");
		table.updateInfo(0, 2, "k");
		//System.out.println("table is " + table);	

		System.out.println("------++++++++++++++++++++++++++++++++++-");
		table.updateInfo(0, 3, "m");
		//System.out.println("table is " + table);
		System.out.println(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>");

		table.updateInfo(4, 5, "y");
		System.out.println("table is " + table);

		
	}

}
